# Developer API & Exports

This document provides a comprehensive guide to the **`tg-hud`** Developer API, Client Exports, and Server Events. These APIs allow external resources (such as inventories, medical scripts, consumable items, minigames, and cinematic cameras) to seamlessly interact with HUD visibility, radar positioning, and player stress levels.

---

## 📑 Table of Contents

1. [Client Exports](#1-client-exports)
   - [HUD Visibility (`toggleHud`)](#hud-visibility-togglehud)
   - [Stop HUD Threads (`stopHud`)](#stop-hud-threads-stophud)
   - [Stress Management Exports](#stress-management-exports)
     - [`getStress`](#getstress)
     - [`gainStress`](#gainstress)
     - [`setStress`](#setstress)
     - [`resetStress`](#resetstress)
   - [Minimap Positioning (`SetRadarPosition`)](#minimap-positioning-setradarposition)
2. [Server Events](#2-server-events)
   - [Update Stress State (`tg-hud:server:updatestress`)](#update-stress-state-tg-hudserverupdatestress)
3. [Integration Examples](#3-integration-examples)
   - [Example 1: Inventory Open / Close Hook](#example-1-inventory-open--close-hook)
   - [Example 2: Consumable Item (Relieving Stress)](#example-2-consumable-item-relieving-stress)
   - [Example 3: Cinematic Camera / Cutscene Sequence](#example-3-cinematic-camera--cutscene-sequence)
4. [Quick Reference Summary Table](#4-quick-reference-summary-table)

---

## 1. Client Exports

All client exports can be called from any client-side Lua script using the standard FiveM export syntax: `exports['tg-hud']:exportName(...)`.

### HUD Visibility (`toggleHud`)
Controls the overall rendering state of all HUD elements (Status vitals, Radar, Speedometer, Info card, Compass, and Weapon HUD).

```lua
-- Explicitly show the HUD
exports['tg-hud']:toggleHud(true)

-- Explicitly hide the HUD
exports['tg-hud']:toggleHud(false)

-- Toggle between visible and hidden
exports['tg-hud']:toggleHud()
```

- **Parameters:**
  - `state` (`boolean` | `nil`, optional):
    - `true`: Forces HUD to become visible.
    - `false`: Forces HUD to hide completely.
    - `nil` (no argument): Inverts the current visibility state.
- **Return Type:** `void`
- **Common Use Cases:**
  - Hiding HUD during inventory menus (`ox_inventory`).
  - Hiding HUD during character creation, clothes stores, or barber shops.
  - Hiding HUD during death, downed states, or hospital screens.

---

### Stop HUD Threads (`stopHud`)
Immediately halts active background update loops (health/armor polling, vehicle telemetry tick loops, and stress timers) and resets internal state caches.

```lua
exports['tg-hud']:stopHud()
```

- **Parameters:** None
- **Return Type:** `void`
- **Common Use Cases:**
  - Calling upon player logout, character switching, or disconnect.
  - Ensuring clean resource unloads without orphan threads running in the background.

---

### Stress Management Exports
`tg-hud` includes a native stress system featuring configurable heartbeat audio, screen blurs, blackouts, and movement stumbling. External resources can query and manipulate stress levels using the following exports:

#### `getStress`
Retrieves the player's current stress level.

```lua
local currentStress = exports['tg-hud']:getStress()
print(('Player current stress: %s'):format(currentStress))
```
- **Parameters:** None
- **Return Type:** `number` (Clamped between `0` and `100`).

#### `gainStress`
Increases the player's stress level by a specified amount.

```lua
-- Add 15 stress points (e.g. after a shootout or vehicle near-miss)
exports['tg-hud']:gainStress(15)
```
- **Parameters:**
  - `amount` (`number`): The quantity of stress points to add.
- **Return Type:** `void`
- **Behavior:** Automatically recalculates total stress, clamps at `100`, and triggers visual screen blur / camera shakes if crossing the configured threshold (`minlevel`).

#### `setStress`
Overrides the player's stress level to an exact value.

```lua
-- Set stress to exactly 50%
exports['tg-hud']:setStress(50)
```
- **Parameters:**
  - `value` (`number`): Desired stress value (`0` to `100`).
- **Return Type:** `void`

#### `resetStress`
Completely clears all stress from the player and stops any active blur or blackout screen effects.

```lua
-- Completely remove all stress
exports['tg-hud']:resetStress()
```
- **Parameters:** None
- **Return Type:** `void`
- **Common Use Cases:**
  - Medical treatments at hospital beds or EMS revivals.
  - Relaxing activities such as yoga, spa, smoking, or consuming medicinal items.

---

### Minimap Positioning (`SetRadarPosition`)
Programmatically updates the GTA V minimap / radar viewport coordinates, size, and mask shape.

```lua
exports['tg-hud']:SetRadarPosition(top, left, height, width, style)
```

- **Parameters:**
  - `top` (`number`): Vertical pixel offset from the top edge of the screen.
  - `left` (`number`): Horizontal pixel offset from the left edge of the screen.
  - `height` (`number`): Minimap viewport height in pixels.
  - `width` (`number`): Minimap viewport width in pixels.
  - `style` (`string`): Radar shape mask:
    - `'rounded'`: Rectangular mask with rounded corners (`tgmap_round`).
    - `'circle'`: Circular mask (`tgmap`).
- **Return Type:** `void`
- **Description:** Adjusts the native GTA radar mask matrices and re-anchors the streamed `minimap.gfx` texture dictionary dynamically without requiring a game restart.

---

## 2. Server Events

### Update Stress State (`tg-hud:server:updatestress`)
Allows server-side scripts (item handlers, database loaders, administrative commands) to update a player's stress level across the network.

```lua
-- Triggered from client to inform server of an update:
TriggerServerEvent('tg-hud:server:updatestress', 45)
```

- **Payload:**
  - `stressLevel` (`number`): Target stress value to synchronize.
- **Server-Side Handling:**
  - Broadcasts or saves the updated value to player state bags or framework user accounts (`xPlayer.setMeta('stress', value)` or `Player.Functions.SetMetaData('stress', value)`).

---

## 3. Integration Examples

### Example 1: Inventory Open / Close Hook
Hide the entire HUD whenever a custom inventory opens and restore it upon closing:

```lua
-- When your inventory UI opens
AddEventHandler('my_inventory:client:opened', function()
    exports['tg-hud']:toggleHud(false)
end)

-- When your inventory UI closes
AddEventHandler('my_inventory:client:closed', function()
    exports['tg-hud']:toggleHud(true)
end)
```

---

### Example 2: Consumable Item (Relieving Stress)
Handle an external usable item (e.g., cigarette, coffee, or prescription medication) that relieves stress over time:

```lua
RegisterNetEvent('consumables:client:smokeCigarette', function()
    local success = lib.progressBar({
        duration = 5000,
        label = 'Smoking cigarette...',
        useWhileDead = false,
        canCancel = true,
        anim = {
            dict = 'amb@world_human_smoking@male@male_a@enter',
            clip = 'enter'
        }
    })

    if success then
        -- Retrieve current stress and reduce by 20 points
        local current = exports['tg-hud']:getStress()
        local newStress = math.max(0, current - 20)
        
        exports['tg-hud']:setStress(newStress)
        lib.notify({
            title = 'Relaxed',
            description = 'You feel much calmer now.',
            type = 'success'
        })
    end
end)
```

---

### Example 3: Cinematic Camera / Cutscene Sequence
Temporarily disable HUD and background processing during an in-game cinematic cutscene:

```lua
local function PlayCinematicIntro()
    -- Hide HUD completely
    exports['tg-hud']:toggleHud(false)
    
    -- Create and activate your script camera
    local cam = CreateCam('DEFAULT_SCRIPTED_CAMERA', true)
    SetCamCoord(cam, -205.0, -1015.0, 35.0)
    PointCamAtCoord(cam, -200.0, -1000.0, 30.0)
    RenderScriptCams(true, true, 2000, true, false)

    Wait(5000)

    -- Tear down camera and restore HUD
    RenderScriptCams(false, true, 1000, true, false)
    DestroyCam(cam, false)
    exports['tg-hud']:toggleHud(true)
end
```

---

## 4. Quick Reference Summary Table

| API Identifier | Scope | Type | Parameters | Description |
|---|---|---|---|---|
| `toggleHud` | Client | Export | `state?: boolean` | Shows, hides, or toggles complete HUD visibility. |
| `stopHud` | Client | Export | None | Halts background tick loops and clears runtime caches. |
| `getStress` | Client | Export | None | Returns the current player stress level (`0 - 100`). |
| `gainStress` | Client | Export | `amount: number` | Adds stress points and triggers blur/screen effects. |
| `setStress` | Client | Export | `value: number` | Overrides stress to an exact value (`0 - 100`). |
| `resetStress` | Client | Export | None | Completely removes stress and stops screen blur effects. |
| `SetRadarPosition` | Client | Export | `top, left, height, width, style` | Re-positions and resizes the GTA radar viewport. |
| `tg-hud:server:updatestress` | Server | Event | `stressLevel: number` | Synchronizes stress updates to server state and database. |

