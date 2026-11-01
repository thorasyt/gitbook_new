# shared/fuel.lua

This document provides a comprehensive breakdown of **`shared/fuel.lua`**. It explains the dynamic fuel script auto-detection system, supported third-party fuel resources, native fallbacks, export retrieval logic, error protection, and guidelines for adding custom fuel integrations.

---

## 📑 Table of Contents

1. [Overview & Architecture](#1-overview--architecture)
2. [Callable Metatable Pattern](#2-callable-metatable-pattern)
3. [Auto-Detection System (`DetectFuelSystem`)](#3-auto-detection-system-detectfuelsystem)
4. [Supported Fuel Resources](#4-supported-fuel-resources)
5. [Fuel Retrieval & Export Mapping (`GetFuel`)](#5-fuel-retrieval--export-mapping-getfuel)
   - [Special Handling by Resource](#special-handling-by-resource)
   - [Clamping & Validation](#clamping--validation)
   - [Error Protection (`pcall`) & Fallback](#error-protection-pcall--fallback)
6. [Supported Fuel Resources Comparison Matrix](#6-supported-fuel-resources-comparison-matrix)
7. [Adding a Custom Fuel Resource](#7-adding-a-custom-fuel-resource)
8. [Full Configuration Source Code](#8-full-configuration-source-code)

---

## 1. Overview & Architecture

In FiveM, different servers use different community fuel scripts (e.g., `LegacyFuel`, `ox_fuel`, `cdn-fuel`, `rcore_fuel`). Each script has its own export names, return signatures, or state implementations.

**`shared/fuel.lua`** serves as a unified abstraction layer for `tg-hud`. It automatically detects whichever fuel script is running on your server in real time and queries its exports safely, requiring zero manual configuration from server owners.

```
┌─────────────────────────────────────────────────────────────┐
│                     Vehicle Speedometer                     │
└──────────────────────────────┬──────────────────────────────┘
                               │ calls fuel(vehicle)
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                      shared/fuel.lua                        │
│        (Detects active resource & executes export)          │
└──────────┬───────────────────┼───────────────────┬──────────┘
           │                   │                   │
           ▼                   ▼                   ▼
    ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
    │  ox_fuel /  │     │ LegacyFuel/ │     │ rcore_fuel/ │
    │Renewed-Fuel │     │  cdn-fuel   │     │   ti_fuel   │
    └──────┬──────┘     └──────┬──────┘     └──────┬──────┘
           │                   │                   │
           └───────────────────┼───────────────────┘
                               ▼
                  Clamped Value (0.0 - 100.0)
```

---

## 2. Callable Metatable Pattern

At the bottom of `shared/fuel.lua`, the `fuel` table is wrapped with an `__call` metamethod:

```lua
local fuel = {}

setmetatable(fuel, {
    __call = function(self, vehicle)
        return GetFuel(vehicle)
    end
})

return fuel
```

### Why this exists:
Developers and HUD threads can treat the returned table directly as a callable function:

```lua
-- Retrieve current fuel percentage for any vehicle entity
local currentFuel = fuel(vehicle)
```

This ensures a clean, lightweight syntax within vehicle telemetry loops while isolating all export resolution logic inside this single module.

---

## 3. Auto-Detection System (`DetectFuelSystem`)

The auto-detection engine iterates through the list of known fuel resources and checks their runtime status via FiveM's native `GetResourceState`:

```lua
local function DetectFuelSystem()
    for _, resource in ipairs(FuelResources) do
        if GetResourceState(resource) == 'started' then
            activeFuel = resource
            return activeFuel
        end
    end

    activeFuel = nil
    return nil
end
```

### Key behaviors:
- **Zero Config Required:** Automatically binds to whichever resource is active when your server boots.
- **Dynamic Adaptability:** If a fuel script is restarted live via console (`ensure cdn-fuel`), the system immediately re-detects it without requiring a HUD restart.
- **Native Fallback:** If none of the 13 supported scripts are running, it returns `nil` and falls back to GTA V base vehicle fuel levels (`GetVehicleFuelLevel(vehicle)`).

---

## 4. Supported Fuel Resources

The module includes out-of-the-box support for 13 popular FiveM fuel scripts:

```lua
local FuelResources = {
    'LegacyFuel',
    'lc_fuel',
    'ps-fuel',
    'lj-fuel',
    'cdn-fuel',
    'hyon_gas_station',
    'okokGasStation',
    'nd_fuel',
    'myFuel',
    'ti_fuel',
    'ox_fuel',
    'Renewed-Fuel',
    'rcore_fuel'
}
```

> [!NOTE]
> The detection iterates in the exact order defined in `FuelResources`. If you run multiple fuel scripts concurrently (e.g., a gas station management script alongside a fuel physics script), place your primary fuel provider earlier in the array.

---

## 5. Fuel Retrieval & Export Mapping (`GetFuel`)

```lua
local function GetFuel(vehicle)
    local fuelSystem = DetectFuelSystem()

    if not fuelSystem then
        return GetVehicleFuelLevel(vehicle)
    end

    local success, fuel = pcall(function()
        if fuelSystem == 'ti_fuel' then
            return exports['ti_fuel']:getFuel(vehicle)

        elseif fuelSystem == 'ox_fuel' or fuelSystem == 'Renewed-Fuel' then
            return GetVehicleFuelLevel(vehicle)

        elseif fuelSystem == 'rcore_fuel' then
            return exports['rcore_fuel']:GetVehicleFuelPercentage(vehicle)

        else
            return exports[fuelSystem]:GetFuel(vehicle)
        end
    end)

    if success and type(fuel) == 'number' then
        return math.max(0, math.min(100, fuel))
    end

    return GetVehicleFuelLevel(vehicle)
end
```

### Special Handling by Resource

1. **`ti_fuel`:**
   - Uses lowercase camelCase export: `exports['ti_fuel']:getFuel(vehicle)`.
2. **`ox_fuel` & `Renewed-Fuel`:**
   - Both resources maintain vehicle fuel levels directly via GTA V entity state bags and synced entity fuel metadata. Calling native `GetVehicleFuelLevel(vehicle)` directly avoids unnecessary Lua export overhead and guarantees 100% accurate synchronization.
3. **`rcore_fuel`:**
   - Uses percentage-specific export: `exports['rcore_fuel']:GetVehicleFuelPercentage(vehicle)`.
4. **Standard Scripts (`LegacyFuel`, `cdn-fuel`, `ps-fuel`, `okokGasStation`, etc.):**
   - Use the standard PascalCase export: `exports[fuelSystem]:GetFuel(vehicle)`.

### Clamping & Validation
```lua
if success and type(fuel) == 'number' then
    return math.max(0, math.min(100, fuel))
end
```
- Ensures the returned fuel value is strictly a numeric data type.
- Clamps the value within `0.0` to `100.0` to prevent visual UI overflow or negative gauge percentages in the speedometer.

### Error Protection (`pcall`) & Fallback
The entire export query is wrapped in Lua's `pcall` (protected call):
- If a third-party fuel script crashes, restarts, or throws an unhandled error inside its export, `pcall` catches the exception without interrupting the HUD thread.
- If `success` is `false` or the export returns `nil`, the system automatically falls back to native `GetVehicleFuelLevel(vehicle)`.

---

## 6. Supported Fuel Resources Comparison Matrix

| Resource Name | Detection Key | Export / Method Used | Author / Origin |
|---|---|---|---|
| **LegacyFuel** | `'LegacyFuel'` | `exports['LegacyFuel']:GetFuel(vehicle)` | InZupiere / Standalone |
| **ox_fuel** | `'ox_fuel'` | `GetVehicleFuelLevel(vehicle)` | Overextended |
| **cdn-fuel** | `'cdn-fuel'` | `exports['cdn-fuel']:GetFuel(vehicle)` | Codine Developments |
| **Renewed-Fuel** | `'Renewed-Fuel'` | `GetVehicleFuelLevel(vehicle)` | Renewed Scripts |
| **rcore_fuel** | `'rcore_fuel'` | `exports['rcore_fuel']:GetVehicleFuelPercentage(vehicle)` | Rcore.cz |
| **ti_fuel** | `'ti_fuel'` | `exports['ti_fuel']:getFuel(vehicle)` | TGIANN Scripts |
| **ps-fuel** | `'ps-fuel'` | `exports['ps-fuel']:GetFuel(vehicle)` | Project Sloth |
| **okokGasStation** | `'okokGasStation'` | `exports['okokGasStation']:GetFuel(vehicle)` | okokScripts |
| **lc_fuel** | `'lc_fuel'` | `exports['lc_fuel']:GetFuel(vehicle)` | Luke Scripts |
| **lj-fuel** | `'lj-fuel'` | `exports['lj-fuel']:GetFuel(vehicle)` | Loljtest |
| **hyon_gas_station**| `'hyon_gas_station'` | `exports['hyon_gas_station']:GetFuel(vehicle)` | Hyon Scripts |
| **nd_fuel** | `'nd_fuel'` | `exports['nd_fuel']:GetFuel(vehicle)` | ND Framework |
| **myFuel** | `'myFuel'` | `exports['myFuel']:GetFuel(vehicle)` | MyScripts |
| **Native GTA V** | *Fallback* | `GetVehicleFuelLevel(vehicle)` | Rockstar Games Base |

---

## 7. Adding a Custom Fuel Resource

If your server uses a custom or unlisted fuel resource (e.g. `'custom_fuel'`), follow these two steps:

### Step 1: Add the resource name to `FuelResources`
Open `shared/fuel.lua` and append your resource name to the table:

```lua
local FuelResources = {
    'custom_fuel', -- [!code ++]
    'LegacyFuel',
    'lc_fuel',
    -- ...
}
```

### Step 2: Add your export pattern to `GetFuel`
If your script uses a non-standard export, add a branch to the `pcall` block:

```lua
local success, fuel = pcall(function()
    if fuelSystem == 'custom_fuel' then -- [!code ++]
        return exports['custom_fuel']:GetVehicleFuel(vehicle) -- [!code ++]
    elseif fuelSystem == 'ti_fuel' then
        return exports['ti_fuel']:getFuel(vehicle)
    -- ...
```

---

## 8. Full Configuration Source Code

```lua
local FuelResources = {
    'LegacyFuel',
    'lc_fuel',
    'ps-fuel',
    'lj-fuel',
    'cdn-fuel',
    'hyon_gas_station',
    'okokGasStation',
    'nd_fuel',
    'myFuel',
    'ti_fuel',
    'ox_fuel',
    'Renewed-Fuel',
    'rcore_fuel'
}

local activeFuel = nil

local function DetectFuelSystem()
    for _, resource in ipairs(FuelResources) do
        if GetResourceState(resource) == 'started' then
            activeFuel = resource
            return activeFuel
        end
    end

    activeFuel = nil
    return nil
end

local function GetFuel(vehicle)
    local fuelSystem = DetectFuelSystem()

    if not fuelSystem then
        return GetVehicleFuelLevel(vehicle)
    end

    local success, fuel = pcall(function()
        if fuelSystem == 'ti_fuel' then
            return exports['ti_fuel']:getFuel(vehicle)

        elseif fuelSystem == 'ox_fuel' or fuelSystem == 'Renewed-Fuel' then
            return GetVehicleFuelLevel(vehicle)

        elseif fuelSystem == 'rcore_fuel' then
            return exports['rcore_fuel']:GetVehicleFuelPercentage(vehicle)

        else
            return exports[fuelSystem]:GetFuel(vehicle)
        end
    end)

    if success and type(fuel) == 'number' then
        return math.max(0, math.min(100, fuel))
    end

    return GetVehicleFuelLevel(vehicle)
end

local fuel = {}

setmetatable(fuel, {
    __call = function(self, vehicle)
        return GetFuel(vehicle)
    end
})

return fuel
```

