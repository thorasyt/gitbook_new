# Resource Integrations

`tg-hud` is designed with a plug-and-play architecture that minimizes configuration overhead. It automatically detects and binds to your server's core framework, inventory, voice system, and vehicle fuel scripts out of the box.

This guide outlines how `tg-hud` bridges with your existing FiveM ecosystem and explains how to configure or customize each integrated system.

---

## 📑 Table of Contents

1. [Architecture Overview](#1-architecture-overview)
2. [Framework Integration](#2-framework-integration)
   - [Qbox (`qbx_core`)](#qbox-qbx_core)
   - [ESX Legacy (`es_extended`)](#esx-legacy-es_extended)
   - [Auto-Detection Mechanism](#auto-detection-mechanism)
3. [Inventory Integration (`ox_inventory`)](#3-inventory-integration-ox_inventory)
   - [Auto-Hiding HUD on Open](#auto-hiding-hud-on-open)
   - [Third-Party Inventories](#third-party-inventories)
4. [Voice System (`pma-voice`)](#4-voice-system-pma-voice)
   - [Proximity Ranges & Talking Indicator](#proximity-ranges--talking-indicator)
   - [Radio & Call Transmissions](#radio--call-transmissions)
5. [Fuel Systems (13 Supported Scripts)](#5-fuel-systems-13-supported-scripts)
   - [Supported Systems](#supported-systems)
   - [Speedometer Sync & Fallbacks](#speedometer-sync--fallbacks)
6. [Integration Matrix Summary](#6-integration-matrix-summary)

---

## 1. Architecture Overview

Rather than forcing server developers to manually install complex bridge scripts or modify code inside core resources, `tg-hud` uses **zero-configuration listeners**:

```
┌─────────────────────────────────────────────────────────────────┐
│                             TG-HUD                              │
├─────────────────┬─────────────────┬──────────────┬──────────────┤
│    Framework    │    Inventory    │    Voice     │     Fuel     │
│  (Qbox / ESX)   │ (ox_inventory)  │ (pma-voice)  │ (13 Scripts) │
└────────┬────────┴────────┬────────┴──────┬───────┴──────┬───────┘
         │                 │               │              │
         ▼                 ▼               ▼              ▼
  • Player Cash/Bank  • Auto-Hide HUD  • Proximity    • Speedo Gauge
  • Job / Gang Card   • Ammo Sync      • Voice Pulse  • Low Fuel Warn
  • Hunger & Thirst   • Clean UI       • Radio Icon   • Native Fallback
```

Whenever an integrated resource starts or emits standard state bag changes and events, `tg-hud` captures the data and updates the React user interface seamlessly.

---

## 2. Framework Integration

`tg-hud` natively supports **Qbox (`qbx_core`)** and **ESX Legacy (`es_extended`)**. The framework adapter automatically pulls player identity data, monetary balances, job titles, and status vitals.

### Qbox (`qbx_core`)
When Qbox is running, `tg-hud` binds to Qbox client events and player state:
- **Player Session Lifecycle:** Listens for `qbx_core:client:playerLoaded` and `QBCore:Client:OnPlayerUnload`.
- **Character Info & Accounts:** Monitors `PlayerData.money.cash` and `PlayerData.money.bank` for live info card updates.
- **Job & Gang:** Updates the info card dynamically on `QBCore:Client:OnJobUpdate` and `QBCore:Client:OnGangUpdate`.
- **Needs & Vitals:** Reads metadata for `hunger` and `thirst` (`PlayerData.metadata['hunger']`, `PlayerData.metadata['thirst']`).

### ESX Legacy (`es_extended`)
When ESX is active, `tg-hud` binds to legacy events and state handlers:
- **Player Session Lifecycle:** Listens for `esx:playerLoaded` and `esx:onPlayerLogout`.
- **Accounts:** Tracks cash and bank balance updates via `esx:setAccountMoney`.
- **Job:** Updates job title and grade dynamically on `esx:setJob`.
- **Needs & Vitals:** Interfaces with `esx_status` (`esx_status:onTick`) to read food and drink status percentages.

### Auto-Detection Mechanism
The framework adapter uses FiveM's native resource state check:

```lua
if GetResourceState('qbx_core') == 'started' then
    Framework = 'qbx'
elseif GetResourceState('es_extended') == 'started' then
    Framework = 'esx'
else
    Framework = 'standalone'
end
```

> [!NOTE]
> If neither framework is found, `tg-hud` operates in **Standalone Mode**. In standalone mode, health, armor, stamina, oxygen, and vehicle telemetry remain fully functional, while financial accounts and hunger/thirst meters are cleanly hidden.

---

## 3. Inventory Integration (`ox_inventory`)

`tg-hud` features native, built-in integration with **[`ox_inventory`](https://github.com/overextended/ox_inventory)**.

### Auto-Hiding HUD on Open
When a player opens their inventory, backpack, trunk, or glovebox:
1. `tg-hud` listens to `ox_inventory` UI state change events.
2. The player status HUD, radar, and speedometer automatically fade out to provide an unobstructed inventory screen.
3. Upon closing the inventory, all HUD components smoothly restore to their previous positions.

```lua
-- Handled internally by tg-hud:
AddEventHandler('ox_inventory:opened', function()
    exports['tg-hud']:toggleHud(false)
end)

AddEventHandler('ox_inventory:closed', function()
    exports['tg-hud']:toggleHud(true)
end)
```

### Third-Party Inventories
If your server uses another inventory (such as `qb-inventory`, `qs-inventory`, or `ps-inventory`), you can easily mirror this behavior by calling the `toggleHud` export:

```lua
-- In your inventory script client/main.lua:
exports['tg-hud']:toggleHud(false) -- Call when inventory opens
exports['tg-hud']:toggleHud(true)  -- Call when inventory closes
```

---

## 4. Voice System (`pma-voice`)

`tg-hud` natively integrates with **[`pma-voice`](https://github.com/AvarianKnight/pma-voice)** for player voice vitals and proximity indicators.

### Proximity Ranges & Talking Indicator
- **Proximity Distance:** The microphone vital icon dynamically reflects the player's active voice range:
  - **Whisper:** Typically `1.5m`
  - **Normal:** Typically `3.0m`
  - **Shout:** Typically `6.0m`
- **Dynamic Voice Pulse:** When `MumbleIsPlayerTalking(playerId)` returns `true`, the microphone icon pulses with an active glow effect matching your selected HUD theme color.

### Radio & Call Transmissions
- Monitors `pma-voice` state bags:
  - `Player(serverId).state.radioActive`: Lights up the radio indicator when transmitting over a walkie-talkie frequency.
  - `Player(serverId).state.callChannel`: Displays call status when the player is active in a phone call.

> [!TIP]
> No configuration is needed for `pma-voice`. As long as `ensure pma-voice` starts before `tg-hud` in your `server.cfg`, all listeners register automatically.

---

## 5. Fuel Systems (13 Supported Scripts)

Fuel level retrieval is fully abstracted via [`shared/fuel.lua`](configuration/fuel.md). When driving, the speedometer fuel gauge queries the active script without requiring manual edits.

### Supported Systems

| Fuel Resource | Integration Method | Automatic Sync |
|---|---|:---:|
| **LegacyFuel** | `exports['LegacyFuel']:GetFuel(vehicle)` | ✅ |
| **ox_fuel** | `GetVehicleFuelLevel(vehicle)` (Entity State) | ✅ |
| **cdn-fuel** | `exports['cdn-fuel']:GetFuel(vehicle)` | ✅ |
| **Renewed-Fuel** | `GetVehicleFuelLevel(vehicle)` (Synced State) | ✅ |
| **rcore_fuel** | `exports['rcore_fuel']:GetVehicleFuelPercentage(vehicle)` | ✅ |
| **ti_fuel** | `exports['ti_fuel']:getFuel(vehicle)` | ✅ |
| **ps-fuel** | `exports['ps-fuel']:GetFuel(vehicle)` | ✅ |
| **okokGasStation** | `exports['okokGasStation']:GetFuel(vehicle)` | ✅ |
| **lc_fuel** | `exports['lc_fuel']:GetFuel(vehicle)` | ✅ |
| **lj-fuel** | `exports['lj-fuel']:GetFuel(vehicle)` | ✅ |
| **hyon_gas_station** | `exports['hyon_gas_station']:GetFuel(vehicle)` | ✅ |
| **nd_fuel** | `exports['nd_fuel']:GetFuel(vehicle)` | ✅ |
| **myFuel** | `exports['myFuel']:GetFuel(vehicle)` | ✅ |
| **Base GTA V** | `GetVehicleFuelLevel(vehicle)` (*Fallback*) | ✅ |

### Speedometer Sync & Fallbacks
- The speedometer fuel level gauge displays real-time values from `0%` to `100%`.
- If an unsupported or proprietary fuel system is detected, the resource safely falls back to native GTA V fuel level logic (`GetVehicleFuelLevel(vehicle)`).
- See the [Fuel Configuration Guide](configuration/fuel.md) to add custom export definitions.

---

## 6. Integration Matrix Summary

| Category | Supported Resource | Auto-Detected | Native Fallback | Notes |
|---|---|:---:|:---:|---|
| **Framework** | Qbox (`qbx_core`) | ✅ | Standalone | Cash, Bank, Job, Gang, Hunger, Thirst |
| **Framework** | ESX (`es_extended`) | ✅ | Standalone | Cash, Bank, Job, `esx_status` needs |
| **Inventory** | `ox_inventory` | ✅ | Always visible | Auto-hides HUD and radar on open |
| **Voice** | `pma-voice` | ✅ | Basic mic | Talking glow, proximity ranges, radio |
| **Fuel** | 13 Supported Scripts | ✅ | `GetVehicleFuelLevel` | Real-time gauge, clamped `0 - 100` |

