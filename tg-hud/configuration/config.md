# TG-HUD: Configuration Guide (`shared/config.lua`)

This document provides a comprehensive, field-by-field breakdown of **`shared/config.lua`**. It explains every setting, data type, default value, internal logic, and how each option affects client and server behavior.

---

## 📑 Table of Contents

1. [Overview & Metatable Pattern](#1-overview--metatable-pattern)
2. [Debug Mode](#2-debug-mode)
3. [Basic Configuration (`basicConfig`)](#3-basic-configuration-basicconfig)
   - [Minimap Shape (`minimap`)](#minimap-shape-minimap)
   - [Performance Profile (`performance`)](#performance-profile-performance)
   - [Clock Mode (`time`)](#clock-mode-time)
   - [Compass Orientation (`composs`)](#compass-orientation-composs)
   - [Minimap Display Condition (`minimapShow`)](#minimap-display-condition-minimapshow)
4. [Default HUD Styles (`defaultHudStyle`)](#4-default-hud-styles-defaulthudstyle)
   - [Player Status Style (`status`)](#player-status-style-status)
   - [Speedometer Style (`speedometer`)](#speedometer-style-speedometer)
   - [Server Info Style (`info`)](#server-info-style-info)
5. [Seatbelt System (`seatbelt`)](#5-seatbelt-system-seatbelt)
   - [Master Switch (`enable`)](#master-switch-enable)
   - [Supported Classes (`class`)](#supported-classes-class)
   - [Emergency Vehicle Exemption (`IngnoreEmergencyVehicle`)](#emergency-vehicle-exemption-ingnoreemergencyvehicle)
   - [Passenger Belt Rule (`disablePassengerBelt`)](#passenger-belt-rule-disablepassengerbelt)
   - [Ejection Velocity Threshold (`minSpeedToEject`)](#ejection-velocity-threshold-minspeedtoeject)
   - [Prevent Vehicle Exit While Buckled (`ExitOnSeatBelt`)](#prevent-vehicle-exit-while-buckled-exitonseatbelt)
   - [Keybind Registration (`keybind`)](#keybind-registration-keybind)
6. [Cruise Control System (`cruise`)](#6-cruise-control-system-cruise)
   - [Master Switch (`enable`)](#cruise-master-switch-enable)
   - [Keybind Registration (`keybind`)](#cruise-keybind-registration-keybind)
7. [Stress Management System (`stress`)](#7-stress-management-system-stress)
   - [Master Switch (`enable`)](#stress-master-switch-enable)
   - [Gunshot Stress Probability (`chance`)](#gunshot-stress-probability-chance)
   - [Effect Trigger Threshold (`minlevel`)](#effect-trigger-threshold-minlevel)
   - [Speed Thresholds (`seatBeltMinSpeed` & `minSpeed`)](#speed-thresholds-seatbeltminspeed--minspeed)
   - [Job & Weapon Whitelists (`whitelistedJobs` & `whitelistedWeapon`)](#job--weapon-whitelists-whitelistedjobs--whitelistedweapon)
   - [Screen Blur Intensities (`blur`)](#screen-blur-intensities-blur)
   - [Screen Blur Intervals (`interval`)](#screen-blur-intervals-interval)
   - [Vehicle Class Stress Matrix (`vehicleClass`)](#vehicle-class-stress-matrix-vehicleclass)
8. [Quick Reference Table](#8-quick-reference-table)

---

## 1. Overview & Metatable Pattern

At the bottom of `shared/config.lua`, the following metatable is defined:

```lua
setmetatable(Config, {
    __call = function(self, name)
        if name == nil then return nil end
        return self[name]
    end
})

return Config
```

### Why this exists:
This gives developers dual access syntax:
- **Function syntax:** `Config('seatbelt')` or `Config('stress')`
- **Table property syntax:** `Config.seatbelt` or `Config.stress`

Both return the identical configuration table or primitive value, ensuring full compatibility across all client and server scripts.

---

## 2. Debug Mode

```lua
DebugMode = false
```

- **Data Type:** `boolean` (`true` | `false`)
- **Default:** `false`
- **Description:** Toggles verbose debug logging across the resource.
- **Client Impact:**
  - When `true`: Calls to `DebugLog()` print formatted data and JSON payloads to the F8 console (e.g. KVP loads, thread intervals, weapon switches, vehicle seat events, and stress increments).
  - When `false`: All debug prints are suppressed, saving CPU cycles. Recommended for live production servers.

---

## 3. Basic Configuration (`basicConfig`)

The `basicConfig` table sets the server defaults for initial player sessions. Players can override these settings locally in the `/edithud` menu; their personal choices are saved to client KVP (`tg_hud:data`).

```lua
basicConfig = {
    minimap = 'rounded',     -- 'rounded' or 'circle'
    performance = 'low',     -- 'low', 'smooth', 'balanced', 'ultra'
    time = 'game',           -- 'game' or 'global'
    composs = 'player',      -- 'player' or 'mouse' 
    minimapShow = 'always',  -- 'always' or 'car'
}
```

### Minimap Shape (`minimap`)
- **Type:** `string`
- **Options:** `'rounded'` | `'circle'`
- **Default:** `'rounded'`
- **How it works:**
  - `'rounded'`: Injects the streamed texture dictionary `tgmap_round` with texture `tg-radar-rounded` using rectangular mask clipping (`clip = 0`).
  - `'circle'`: Injects the streamed texture dictionary `tgmap` with texture `tg-radar-circular` using circular mask clipping (`clip = 1`, `square = true`).
  - Both textures stream directly from the `stream/` directory via `minimap.gfx`.

### Performance Profile (`performance`)
- **Type:** `string`
- **Options:** `'low'` | `'smooth'` | `'balanced'` | `'ultra'`
- **Default:** `'low'`
- **How it works:** Governs the loop delay (`Wait()`) of client threads across status updates, vehicle data, and speedometers:

| Profile | Ped Status Thread | Vehicle Status Thread | Speedometer Thread | Target User / Hardware |
|---|---|---|---|---|
| `'ultra'` | 150 ms | 100 ms | 50 ms | High-end PCs seeking maximum fluid animation (60+ FPS gauges) |
| `'balanced'` | 300 ms | 250 ms | 70 ms | Great visual smoothness with balanced CPU usage |
| `'smooth'` | 450 ms | 500 ms | 100 ms | Fluid gameplay with reduced tick frequency |
| `'low'` | 600 ms | 700 ms | 150 ms | Lowest possible CPU footprint, ideal for budget PCs or high player counts |

### Clock Mode (`time`)
- **Type:** `string`
- **Options:** `'game'` | `'global'`
- **Default:** `'game'`
- **How it works:**
  - `'game'`: Reads in-game world time via native `GetClockHours()` and `GetClockMinutes()`.
  - `'global'`: Reads the player's real-world computer time via native `GetLocalTime()`.

### Compass Orientation (`composs`)
- **Type:** `string`
- **Options:** `'player'` | `'mouse'`
- **Default:** `'player'`
- **How it works:**
  - `'player'`: Calculates compass heading from the player ped's physical entity orientation (`GetEntityHeading(cache.ped)`).
  - `'mouse'`: Calculates compass heading from the third-person or first-person camera rotation (`GetGameplayCamRot(0).z`), rotating as the player looks around.

### Minimap Display Condition (`minimapShow`)
- **Type:** `string`
- **Options:** `'always'` | `'car'`
- **Default:** `'always'`
- **How it works:**
  - `'always'`: Minimap is visible at all times (on foot and in vehicles).
  - `'car'`: Minimap automatically hides when on foot and only appears when entering a vehicle.
  - *Note:* Opening the inventory (`ox_inventory`) or enabling Cinematic Mode always suppresses the minimap regardless of this setting.

---

## 4. Default HUD Styles (`defaultHudStyle`)

Sets the default appearance assigned to players when they connect for the first time.

```lua
defaultHudStyle = {
    status = 'neon_horseshoe_arc',
    speedometer = 3,
    info = 4
}
```

### Player Status Style (`status`)
- **Type:** `string`
- **Default:** `'neon_horseshoe_arc'`
- **Available Styles (24 total):**
  - **Square Category:**
    - `square_solid_card`
    - `square_border_ring`
    - `square_liquid_fill`
    - `square_dashed`
    - `square_pie_fill`
    - `neon_arch`
    - `neon_tall_portal`
    - `square_dual_split`
  - **Circle Category:**
    - `circle_solid_disc`
    - `circle_radial_progress`
    - `circle_liquid_fill`
    - `circle_dashed_ring`
    - `circle_pie_fill`
    - `circle_neon_arch`
    - `neon_horseshoe_arc`
    - `circle_dual_split`
  - **Minimal / Linear Category:**
    - `full_labeled_bar`
    - `squircle_badge_percent`
    - `minimal_icon_percent`
    - `capsule_pill_badge`
    - `capsule_slider_bar`
    - `minimal_linear_bar`
    - `underline_bar_stack`
    - `vertical_pillar_bar`

### Speedometer Style (`speedometer`)
- **Type:** `integer` (`1` to `6`)
- **Default:** `3`
- **Description:** Default vehicle speedometer layout (Styles 1 through 6).

### Server Info Style (`info`)
- **Type:** `integer` (`1` to `5`)
- **Default:** `4`
- **Description:** Default layout for the server/character info card displaying ID, Name, Cash, Bank, Job, and Gang.

---

## 5. Seatbelt System (`seatbelt`)

Handles seatbelt logic, crash detection, and windscreen ejection physics.

```lua
seatbelt = {
    enable = true,
    class = {0, 1, 2, 3, 4, 5, 6, 7, 9, 10, 11, 12, 17, 18, 19, 20},
    IngnoreEmergencyVehicle = true,
    disablePassengerBelt = false,
    minSpeedToEject = 40.0,
    ExitOnSeatBelt = false,
    keybind = 'B' 
}
```

### Master Switch (`enable`)
- **Type:** `boolean`
- **Default:** `true`
- **Description:** If set to `false`, disables seatbelt keybinds, collision monitoring threads, and ejection physics entirely.

### Supported Classes (`class`)
- **Type:** `table` (array of integers)
- **Description:** GTA V vehicle class IDs that support seatbelts:
  - `0`: Compacts | `1`: Sedans | `2`: SUVs | `3`: Coupes | `4`: Muscle | `5`: Sports Classics
  - `6`: Sports | `7`: Super | `9`: Off Road | `10`: Industrial | `11`: Utility | `12`: Vans
  - `17`: Service | `18`: Emergency | `19`: Military | `20`: Commercial
  - *Excluded by default:* `8` (Motorcycles), `13` (Cycles), `14` (Boats), `15` (Helicopters), `16` (Planes), `21` (Trains).

### Emergency Vehicle Exemption (`IngnoreEmergencyVehicle`)
- **Type:** `boolean`
- **Default:** `true`
- **Description:** When `true`, vehicles belonging to Class `18` (Police, EMS, Fire) bypass seatbelt requirements. First responders will not be ejected during high-speed collisions even without buckling.

### Passenger Belt Rule (`disablePassengerBelt`)
- **Type:** `boolean`
- **Default:** `false`
- **Description:**
  - `false`: Passengers require seatbelts and can be ejected if unbuckled during a crash.
  - `true`: Only the driver (seat index `-1`) requires a seatbelt.

### Ejection Velocity Threshold (`minSpeedToEject`)
- **Type:** `number` (float)
- **Default:** `40.0` (in MPH)
- **Description:** The minimum speed before a sudden collision can launch unbuckled occupants through the windscreen.
- **Physics Calculation:**
  - Converted internally to m/s (`minSpeedToEject / 2.237`).
  - Monitors forward speed vectors (`GetEntitySpeedVector(vehicle, true).y`).
  - Triggers if speed drops suddenly by >25% on collision with an obstacle or >35% on extreme deceleration.
  - Smashes vehicle front windows (`4` & `6`), ragdolls the player, applies launch velocity, and inflicts health damage proportional to speed (`speedMph * 0.4`).

### Prevent Vehicle Exit While Buckled (`ExitOnSeatBelt`)
- **Type:** `boolean`
- **Default:** `false`
- **Description:**
  - `false`: Players are prevented from jumping out of their vehicle while buckled (`DisableControlAction(0, 75, true)` disables the F / exit vehicle key). They must press `B` to unbuckle first.
  - `true`: Players can exit freely; getting out automatically clears seatbelt state.

### Keybind Registration (`keybind`)
- **Type:** `string`
- **Default:** `'B'`
- **Description:** Default keyboard key mapped via `RegisterKeyMapping("toggle_seatbelt")`. Players can rebind this key in GTA V Settings -> Key Bindings -> FiveM.

---

## 6. Cruise Control System (`cruise`)

```lua
cruise = {
    enable = true,
    keybind = 'J'
}
```

### Master Switch (`enable`)
- **Type:** `boolean`
- **Default:** `true`
- **Description:** When `true`, enables the cruise control command and speed-holding loop.

### Keybind Registration (`keybind`)
- **Type:** `string`
- **Default:** `'J'`
- **How Cruise Control Works:**
  - Active only for driver (`seat -1`) in land vehicles with the engine running.
  - Minimum activation speed: 3.0 m/s (~11 km/h).
  - Automatically cancels on:
    - Pressing foot brake (`Control 72`) or handbrake (`Control 76`).
    - Vehicle drifting/sliding (>25 degree angle between forward vector and velocity vector).
    - Engine turning off or driver exiting.
  - **Speed Adjust:** Pressing the cruise keybind again while cruise is active resets the cruise speed to the current driving speed.

---

## 7. Stress Management System (`stress`)

Handles player stress accumulation, job/weapon whitelisting, tiered visual screen blur, and extreme ragdoll fall effects.

```lua
stress = {
    enable = true,
    chance = 0.1,
    minlevel = 50,
    seatBeltMinSpeed = 100,
    minSpeed = 40,
    disableForPolice = false,
    whitelistedWeapon = {
        "weapon_petrolcan", 
        "weapon_hazardcan",
        "weapon_fireextinguisher",
    },
    whitelistedJobs = {'police'},
    blur = {
        [1] = {min = 50, max = 59, intensity = 1500},
        [2] = {min = 60, max = 69, intensity = 2000},
        [3] = {min = 70, max = 79, intensity = 2500},
        [4] = {min = 80, max = 89, intensity = 2700},
        [5] = {min = 90, max = 100, intensity = 3000},
    },
    interval = {
        [1] = {min = 50, max = 59, timeout = math.random(50000, 60000)},
        [2] = {min = 60, max = 69, timeout = math.random(40000, 50000)},
        [3] = {min = 70, max = 79, timeout = math.random(30000, 40000)},
        [4] = {min = 80, max = 89, timeout = math.random(20000, 30000)},
        [5] = {min = 90, max = 100, timeout = math.random(15000, 20000)},
    },
    vehicleClass = {
        ['0'] = true,  -- Compacts
        ['1'] = true,  -- Sedans
        ['2'] = true,  -- SUVs
        ['3'] = true,  -- Coupes
        ['4'] = true,  -- Muscle
        ['5'] = true,  -- Sports Classics
        ['6'] = true,  -- Sports
        ['7'] = true,  -- Super
        ['8'] = false, -- Motorcycles
        ['9'] = true,  -- Off Road
        ['10'] = true, -- Industrial
        ['11'] = true, -- Utility
        ['12'] = true, -- Vans
        ['13'] = false,-- Cycles
        ['14'] = false,-- Boats
        ['15'] = false,-- Helicopters
        ['16'] = false,-- Planes
        ['18'] = false,-- Emergency
        ['19'] = false,-- Military
        ['20'] = false,-- Commercial
        ['21'] = false -- Trains
    }
}
```

### Master Switch (`enable`)
- **Type:** `boolean`
- **Default:** `true`
- **Description:** If set to `false`, sets `LocalPlayer.state.stress` to 0 and stops all stress-related threads and effects.

### Gunshot Stress Probability (`chance`)
- **Type:** `number` (float, `0.0` to `1.0`)
- **Default:** `0.1` (10% chance)
- **Description:** The likelihood of gaining stress when discharging a non-whitelisted firearm. When triggered, the player gains a random amount between `1` and `5` stress.

### Effect Trigger Threshold (`minlevel`)
- **Type:** `integer`
- **Default:** `50`
- **Description:** Minimum stress level before visual screen blur effects begin triggering. Any stress below this value has no negative visual impairment.

### Speed Thresholds (`seatBeltMinSpeed` & `minSpeed`)
- **`minSpeed = 40` (in KM/H):** Speed required to accumulate driving stress when **NOT** wearing a seatbelt.
- **`seatBeltMinSpeed = 100` (in KM/H):** Speed required to accumulate driving stress when **WEARING** a seatbelt.
- *Logic:* Driving unbuckled stresses players out at much lower speeds. While buckled, players can cruise up to 99 km/h before driving stress begins accumulating (1 to 3 points per second).

### Job & Weapon Whitelists (`whitelistedJobs` & `whitelistedWeapon`)
- **`whitelistedJobs = {'police'}`:** Players with this job name never gain stress from high-speed driving or shooting. Add other jobs like `'ambulance'`, `'sheriff'` as needed.
- **`whitelistedWeapon`:** Utility items that do not inflict gunshot stress when discharged (`weapon_petrolcan`, `weapon_hazardcan`, `weapon_fireextinguisher`).

### Screen Blur Intensities (`blur`)
- **Type:** Array of range tables (`min`, `max`, `intensity` in ms)
- **Description:** Controls how long the screen blur persists during a stress pulse:
  - **Level 50 - 59:** Blur lasts **1.5 seconds** (`1500ms`)
  - **Level 60 - 69:** Blur lasts **2.0 seconds** (`2000ms`)
  - **Level 70 - 79:** Blur lasts **2.5 seconds** (`2500ms`)
  - **Level 80 - 89:** Blur lasts **2.7 seconds** (`2700ms`)
  - **Level 90 - 100:** Blur lasts **3.0 seconds** (`3000ms`)

### Screen Blur Intervals (`interval`)
- **Type:** Array of range tables (`min`, `max`, `timeout` in ms)
- **Description:** Randomized cooldown period between screen blur pulses:
  - **Level 50 - 59:** Pulses every **50 to 60 seconds**
  - **Level 60 - 69:** Pulses every **40 to 50 seconds**
  - **Level 70 - 79:** Pulses every **30 to 40 seconds**
  - **Level 80 - 89:** Pulses every **20 to 30 seconds**
  - **Level 90 - 100:** Pulses every **15 to 20 seconds**

> [!WARNING]
> **Stress Level 100 Penalty:** When stress reaches 100, `ApplyFullStressEffect()` is triggered. The player is forced into a ragdoll stumble/fall animation on foot, and their screen executes 2 to 4 blackout fade cycles (`DoScreenFadeOut(200)` and `DoScreenFadeIn(200)`), simulating an anxiety blackout.

### Vehicle Class Stress Matrix (`vehicleClass`)
- **Type:** Table of stringified vehicle class indices (`'0'` through `'21'`) with boolean flags.
- **`true`:** Driving this class at or above the speed threshold generates driving stress.
- **`false`:** Driving this class does NOT generate stress (e.g. motorcycles, bicycles, boats, planes, emergency vehicles).

---

## 8. Quick Reference Table

| Setting | Path in Config | Default Value | Recommended Use |
|---|---|---|---|
| Debug Logging | `Config.DebugMode` | `false` | Leave `false` on live servers; `true` for development |
| Minimap Mask | `Config.basicConfig.minimap` | `'rounded'` | Set `'rounded'` for modern look, `'circle'` for classic circular |
| Performance Tier | `Config.basicConfig.performance` | `'low'` | `'low'` or `'smooth'` for large servers; `'ultra'` for showcase recordings |
| Clock Source | `Config.basicConfig.time` | `'game'` | `'game'` for RP immersion, `'global'` for real-world time |
| Compass Mode | `Config.basicConfig.composs` | `'player'` | `'player'` tracks ped direction; `'mouse'` tracks camera look angle |
| Minimap Visibility | `Config.basicConfig.minimapShow` | `'always'` | `'car'` if you prefer minimap visible only inside vehicles |
| Default Status Style | `Config.defaultHudStyle.status` | `'neon_horseshoe_arc'` | Any of the 24 available template IDs |
| Default Speedometer | `Config.defaultHudStyle.speedometer` | `3` | Layout integer from `1` to `6` |
| Default Info Card | `Config.defaultHudStyle.info` | `4` | Layout integer from `1` to `5` |
| Seatbelt Active | `Config.seatbelt.enable` | `true` | Enables seatbelt mechanics and windscreen ejection |
| Lock Exit When Buckled | `Config.seatbelt.ExitOnSeatBelt` | `false` | Set `false` for realistic RP (players must unbuckle before leaving) |
| Ejection Speed | `Config.seatbelt.minSpeedToEject` | `40.0` | Minimum speed (MPH) to trigger windscreen smash |
| Cruise Control Active | `Config.cruise.enable` | `true` | Allows drivers to lock speed using keybind |
| Stress Active | `Config.stress.enable` | `true` | Activates stress accumulation and blur penalties |
| Shooting Stress Chance | `Config.stress.chance` | `0.1` | `0.1` = 10% chance per gunshot fired |
| Blur Threshold | `Config.stress.minlevel` | `50` | Blur starts at level 50; falls/blackouts at level 100 |
| Seatbelt Speed Limit | `Config.stress.seatBeltMinSpeed` | `100` | Speed (KM/H) before fast driving causes stress with seatbelt |
| Unbuckled Speed Limit | `Config.stress.minSpeed` | `40` | Speed (KM/H) before fast driving causes stress without seatbelt |

