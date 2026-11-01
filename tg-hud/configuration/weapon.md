# shared/weapon.lua

This document provides a comprehensive breakdown of **`shared/weapon.lua`**. It details the pre-hashed weapon registry, human-readable label resolution, technical name mapping for UI icon assets, metatable access patterns, and step-by-step instructions for registering custom addon weapons.

---

## 📑 Table of Contents

1. [Overview & Architecture](#1-overview--architecture)
2. [Pre-Hashing & Performance Pattern (`joaat`)](#2-pre-hashing--performance-pattern-joaat)
3. [Callable Metatable Pattern](#3-callable-metatable-pattern)
4. [Weapon Categories Breakdown](#4-weapon-categories-breakdown)
   - [Melee Weapons](#melee-weapons)
   - [Handguns & Pistols](#handguns--pistols)
   - [Submachine Guns (SMG) & Machine Pistols](#submachine-guns-smg--machine-pistols)
   - [Shotguns](#shotguns)
   - [Assault Rifles](#assault-rifles)
   - [Light Machine Guns (LMG)](#light-machine-guns-lmg)
   - [Sniper & Precision Rifles](#sniper--precision-rifles)
   - [Heavy Weapons & Launchers](#heavy-weapons--launchers)
   - [Throwables & Explosives](#throwables--explosives)
   - [Utility & Miscellaneous](#utility--miscellaneous)
   - [Mk II Weapon Variants](#mk-ii-weapon-variants)
5. [Weapon Catalog Reference Table](#5-weapon-catalog-reference-table)
6. [Adding Custom / Addon Weapons](#6-adding-custom--addon-weapons)
7. [Full Configuration Source Code](#7-full-configuration-source-code)

---

## 1. Overview & Architecture

When a player equips, aims, or fires a weapon in FiveM, native functions such as `GetSelectedPedWeapon(playerPed)` or `cache.weapon` return a 32-bit signed/unsigned integer hash (e.g., `453432689` for `WEAPON_PISTOL`).

**`shared/weapon.lua`** serves as the central weapon lookup dictionary for `tg-hud`:
1. **Human-Readable Labels:** Converts raw weapon hashes into clean names for the HUD (e.g., `"Combat Pistol"`).
2. **NUI Icon Resolution:** Supplies the exact string identifier (e.g., `"WEAPON_COMBATPISTOL"`) so the React frontend can dynamically load the matching weapon SVG/PNG icon.

```
┌─────────────────────────────────────────────────────────────┐
│                      Player Ped State                       │
└──────────────────────────────┬──────────────────────────────┘
                               │ cache.weapon (hash integer)
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                     shared/weapon.lua                       │
│                   WeaponList(weaponHash)                    │
└──────────────────────────────┬──────────────────────────────┘
                               │ returns label, value
                               ▼
┌──────────────────────────────┴──────────────────────────────┐
│                       React Weapon HUD                      │
│   • Text Label:  "Combat Pistol"                            │
│   • Icon Asset:  icons/weapons/WEAPON_COMBATPISTOL.svg      │
│   • Ammo & Clip: 12 / 250                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Pre-Hashing & Performance Pattern (`joaat`)

Rather than calculating string hashes repeatedly inside the tick loop every frame, `weapon.lua` pre-computes Jenkins one-at-a-time (`joaat`) hashes once during resource initialization:

```lua
local WeaponList = {}

for k, v in pairs(WeaponNames) do
    local hash = joaat(k)
    WeaponList[hash] = { label = v, value = k }
end
```

### Benefits:
- **$O(1)$ Hash Lookups:** Direct table index by integer hash with zero CPU overhead.
- **Zero Redundant Hashes:** `joaat()` is only invoked once per weapon when the script starts.
- **Unified Payload:** Stores both `{ label = "Baseball Bat", value = "WEAPON_BAT" }` under the computed hash.

---

## 3. Callable Metatable Pattern

At the bottom of `shared/weapon.lua`, a metatable with an `__call` metamethod is attached to `WeaponList`:

```lua
setmetatable(WeaponList, {
    __call = function(self, hash)
        if not hash then return end
        local value = self[hash]
        if not value then return nil, nil end
        return value.label, value.value
    end
})

return WeaponList
```

### Why this exists:
This grants developers dual return values in a single clean function call:

```lua
-- Retrieve both friendly name and icon key in one line
local label, name = WeaponList(weaponHash)

-- Example return:
-- label = "Carbine Rifle Mk II"
-- name  = "WEAPON_CARBINERIFLE_MK2"
```

If an unknown or unarmed hash (`WEAPON_UNARMED` / `0`) is queried, it returns `nil` safely without throwing runtime errors.

---

## 4. Weapon Categories Breakdown

The default configuration includes **87 base GTA V weapons**:

### Melee Weapons
- **Standard:** Knife (`WEAPON_KNIFE`), Nightstick (`WEAPON_NIGHTSTICK`), Hammer (`WEAPON_HAMMER`), Baseball Bat (`WEAPON_BAT`), Golf Club (`WEAPON_GOLFCLUB`), Crowbar (`WEAPON_CROWBAR`).
- **Blades & Edged:** Antique Cavalry Dagger (`WEAPON_DAGGER`), Switchblade (`WEAPON_SWITCHBLADE`), Battle Axe (`WEAPON_BATTLEAXE`), Stone Hatchet (`WEAPON_STONE_HATCHET`).
- **Blunt & Tools:** Pool Cue (`WEAPON_POOLCUE`), Wrench (`WEAPON_WRENCH`), Pipe Wrench (`WEAPON_PIPEWRENCH`), Flashlight (`WEAPON_FLASHLIGHT`).

### Handguns & Pistols
- **Standard Pistols:** Pistol (`WEAPON_PISTOL`), Combat Pistol (`WEAPON_COMBATPISTOL`), AP Pistol (`WEAPON_APPISTOL`), Pistol .50 (`WEAPON_PISTOL50`).
- **Classic & Heavy:** Vintage Pistol (`WEAPON_VINTAGEPISTOL`), Heavy Revolver (`WEAPON_REVOLVER`), Double-Action Revolver (`WEAPON_DOUBLEACTION`), Marksman Pistol (`WEAPON_MARKSMANPISTOL`), Navy Revolver (`WEAPON_NAVYREVOLVER`), Ceramic Pistol (`WEAPON_CERAMICPISTOL`), Perico Pistol (`WEAPON_GADGETPISTOL`).
- **Specialty:** Stun Gun (`WEAPON_STUNGUN`), Flare Gun (`WEAPON_FLAREGUN`), Up-n-Atomizer (`WEAPON_RAYPISTOL`).

### Submachine Guns (SMG) & Machine Pistols
- Micro SMG (`WEAPON_MICROSMG`), SMG (`WEAPON_SMG`), Assault SMG (`WEAPON_ASSAULTSMG`), Mini SMG (`WEAPON_MINISMG`), Machine Pistol (`WEAPON_MACHINEPISTOL`).

### Shotguns
- Pump Shotgun (`WEAPON_PUMPSHOTGUN`), Sawed-Off Shotgun (`WEAPON_SAWNOFFSHOTGUN`), Assault Shotgun (`WEAPON_ASSAULTSHOTGUN`), Bullpup Shotgun (`WEAPON_BULLPUPSHOTGUN`), Heavy Shotgun (`WEAPON_HEAVYSHOTGUN`), Combat Shotgun (`WEAPON_COMBATSHOTGUN`).

### Assault Rifles
- Assault Rifle (`WEAPON_ASSAULTRIFLE`), Carbine Rifle (`WEAPON_CARBINERIFLE`), Advanced Rifle (`WEAPON_ADVANCEDRIFLE`), Musket (`WEAPON_MUSKET`), Compact Rifle (`WEAPON_COMPACTRIFLE`), Military Rifle (`WEAPON_MILITARYRIFLE`), Heavy Rifle (`WEAPON_HEAVYRIFLE`), Service Carbine / Tactical Rifle (`WEAPON_TACTICALRIFLE`).

### Light Machine Guns (LMG)
- MG (`WEAPON_MG`), Combat MG (`WEAPON_COMBATMG`), Unholy Hellbringer (`WEAPON_RAYCARBINE`).

### Sniper & Precision Rifles
- Sniper Rifle (`WEAPON_SNIPERRIFLE`), Heavy Sniper (`WEAPON_HEAVYSNIPER`), Marksman Rifle (`WEAPON_MARKSMANRIFLE`), Precision Rifle (`WEAPON_PRECISIONRIFLE`).

### Heavy Weapons & Launchers
- Grenade Launcher (`WEAPON_GRENADELAUNCHER`), RPG (`WEAPON_RPG`), Minigun (`WEAPON_MINIGUN`), Firework Launcher (`WEAPON_FIREWORK`), Homing Launcher (`WEAPON_HOMINGLAUNCHER`), Compact Grenade Launcher (`WEAPON_COMPACTLAUNCHER`), Compact EMP Launcher (`WEAPON_EMPLAUNCHER`), Widowmaker (`WEAPON_RAYMINIGUN`).

### Throwables & Explosives
- Grenade (`WEAPON_GRENADE`), Sticky Bomb (`WEAPON_STICKYBOMB`), Smoke Grenade (`WEAPON_SMOKEGRENADE`), BZ Gas (`WEAPON_BZGAS`), Molotov (`WEAPON_MOLOTOV`), Proximity Mine (`WEAPON_PROXMINE`), Pipe Bomb (`WEAPON_PIPEBOMB`), Flare (`WEAPON_FLARE`), Snowball (`WEAPON_SNOWBALL`), Ball (`WEAPON_BALL`).

### Utility & Miscellaneous
- Fire Extinguisher (`WEAPON_FIREEXTINGUISHER`), Petrol Can (`WEAPON_PETROLCAN`), Garbage Bag (`WEAPON_GARBAGEBAG`), Handcuffs (`WEAPON_HANDCUFFS`).

### Mk II Weapon Variants
Upgraded weapon bench variants with enhanced attachments and ammunition types:
- **Pistols:** Pistol Mk II (`WEAPON_PISTOL_MK2`), SNS Pistol Mk II (`WEAPON_SNSPISTOL_MK2`), Heavy Revolver Mk II (`WEAPON_REVOLVER_MK2`).
- **SMG:** SMG Mk II (`WEAPON_SMG_MK2`).
- **Shotguns:** Pump Shotgun Mk II (`WEAPON_PUMPSHOTGUN_MK2`).
- **Rifles:** Assault Rifle Mk II (`WEAPON_ASSAULTRIFLE_MK2`), Carbine Rifle Mk II (`WEAPON_CARBINERIFLE_MK2`), Special Carbine Mk II (`WEAPON_SPECIALCARBINE_MK2`), Bullpup Rifle Mk II (`WEAPON_BULLPUPRIFLE_MK2`).
- **Heavy & Sniper:** Combat MG Mk II (`WEAPON_COMBATMG_MK2`), Heavy Sniper Mk II (`WEAPON_HEAVYSNIPER_MK2`).

---

## 5. Weapon Catalog Reference Table

| Category | Weapon Key | Display Label |
|---|---|---|
| **Pistols** | `WEAPON_PISTOL` | Pistol |
| **Pistols** | `WEAPON_COMBATPISTOL` | Combat Pistol |
| **Pistols** | `WEAPON_APPISTOL` | AP Pistol |
| **Pistols** | `WEAPON_PISTOL50` | Pistol .50 |
| **Pistols** | `WEAPON_STUNGUN` | Stun Gun |
| **Pistols** | `WEAPON_REVOLVER` | Heavy Revolver |
| **Pistols** | `WEAPON_PISTOL_MK2` | Pistol Mk II |
| **SMG** | `WEAPON_MICROSMG` | Micro SMG |
| **SMG** | `WEAPON_SMG` | SMG |
| **SMG** | `WEAPON_ASSAULTSMG` | Assault SMG |
| **SMG** | `WEAPON_SMG_MK2` | SMG Mk II |
| **Rifles** | `WEAPON_ASSAULTRIFLE` | Assault Rifle |
| **Rifles** | `WEAPON_CARBINERIFLE` | Carbine Rifle |
| **Rifles** | `WEAPON_ADVANCEDRIFLE` | Advanced Rifle |
| **Rifles** | `WEAPON_SPECIALCARBINE_MK2` | Special Carbine Mk II |
| **Rifles** | `WEAPON_TACTICALRIFLE` | Service Carbine |
| **Shotguns** | `WEAPON_PUMPSHOTGUN` | Pump Shotgun |
| **Shotguns** | `WEAPON_SAWNOFFSHOTGUN` | Sawed-Off Shotgun |
| **Shotguns** | `WEAPON_ASSAULTSHOTGUN` | Assault Shotgun |
| **Snipers** | `WEAPON_SNIPERRIFLE` | Sniper Rifle |
| **Snipers** | `WEAPON_HEAVYSNIPER` | Heavy Sniper |
| **Snipers** | `WEAPON_HEAVYSNIPER_MK2` | Heavy Sniper Mk II |
| **Heavy** | `WEAPON_RPG` | RPG |
| **Heavy** | `WEAPON_MINIGUN` | Minigun |
| **Heavy** | `WEAPON_RAYMINIGUN` | Widowmaker |

---

## 6. Adding Custom / Addon Weapons

If your server features custom weapon models (e.g. custom Glock, AR-15, Katana), you can easily register them into `shared/weapon.lua`:

### Step 1: Add Weapon to `WeaponNames` Table
Open `shared/weapon.lua` and append your custom weapon spawn code and label:

```lua
local WeaponNames = {
  WEAPON_GLOCK19 = "Glock 19 Gen 5",       -- [!code ++]
  WEAPON_AR15    = "Colt AR-15 Custom",    -- [!code ++]
  WEAPON_KATANA  = "Japanese Katana",      -- [!code ++]
  WEAPON_KNIFE   = "Knife",
  -- ...
}
```

### Step 2: Add Matching Icon Asset (NUI)
Place the matching SVG or PNG icon inside the HUD's web UI asset directory using the exact spawn key name:
```
web/public/icons/weapons/WEAPON_GLOCK19.svg
```

### Step 3: Restart Resource
Restart `tg-hud` or refresh your server. The loop automatically executes `joaat('WEAPON_GLOCK19')` and maps the new weapon seamlessly.

---

## 7. Full Configuration Source Code

```lua
local WeaponNames = {
  WEAPON_KNIFE = "Knife",
  WEAPON_NIGHTSTICK = "Nightstick",
  WEAPON_HAMMER = "Hammer",
  WEAPON_BAT = "Baseball Bat",
  WEAPON_GOLFCLUB = "Golf Club",
  WEAPON_CROWBAR = "Crowbar",
  WEAPON_PISTOL = "Pistol",
  WEAPON_COMBATPISTOL = "Combat Pistol",
  WEAPON_APPISTOL = "AP Pistol",
  WEAPON_PISTOL50 = "Pistol .50",
  WEAPON_MICROSMG = "Micro SMG",
  WEAPON_SMG = "SMG",
  WEAPON_ASSAULTSMG = "Assault SMG",
  WEAPON_ASSAULTRIFLE = "Assault Rifle",
  WEAPON_CARBINERIFLE = "Carbine Rifle",
  WEAPON_ADVANCEDRIFLE = "Advanced Rifle",
  WEAPON_MG = "MG",
  WEAPON_COMBATMG = "Combat MG",
  WEAPON_PUMPSHOTGUN = "Pump Shotgun",
  WEAPON_SAWNOFFSHOTGUN = "Sawed-Off Shotgun",
  WEAPON_ASSAULTSHOTGUN = "Assault Shotgun",
  WEAPON_BULLPUPSHOTGUN = "Bullpup Shotgun",
  WEAPON_STUNGUN = "Stun Gun",
  WEAPON_SNIPERRIFLE = "Sniper Rifle",
  WEAPON_HEAVYSNIPER = "Heavy Sniper",
  WEAPON_GRENADELAUNCHER = "Grenade Launcher",
  WEAPON_RPG = "RPG",
  WEAPON_MINIGUN = "Minigun",
  WEAPON_GRENADE = "Grenade",
  WEAPON_STICKYBOMB = "Sticky Bomb",
  WEAPON_SMOKEGRENADE = "Smoke Grenade",
  WEAPON_BZGAS = "BZ Gas",
  WEAPON_MOLOTOV = "Molotov",
  WEAPON_FIREEXTINGUISHER = "Fire Extinguisher",
  WEAPON_PETROLCAN = "Petrol Can",
  WEAPON_BALL = "Ball",
  WEAPON_FLARE = "Flare",
  WEAPON_SNOWBALL = "Snowball",
  WEAPON_FLAREGUN = "Flare Gun",
  WEAPON_GARBAGEBAG = "Garbage Bag",
  WEAPON_HANDCUFFS = "Handcuffs",
  WEAPON_BATTLEAXE = "Battle Axe",
  WEAPON_COMPACTLAUNCHER = "Compact Grenade Launcher",
  WEAPON_MINISMG = "Mini SMG",
  WEAPON_PIPEBOMB = "Pipe Bomb",
  WEAPON_POOLCUE = "Pool Cue",
  WEAPON_WRENCH = "Wrench",
  WEAPON_FLASHLIGHT = "Flashlight",
  WEAPON_DAGGER = "Antique Cavalry Dagger",
  WEAPON_VINTAGEPISTOL = "Vintage Pistol",
  WEAPON_MUSKET = "Musket",
  WEAPON_FIREWORK = "Firework Launcher",
  WEAPON_MARKSMANRIFLE = "Marksman Rifle",
  WEAPON_HEAVYSHOTGUN = "Heavy Shotgun",
  WEAPON_PROXMINE = "Proximity Mine",
  WEAPON_HOMINGLAUNCHER = "Homing Launcher",
  WEAPON_COMPACTRIFLE = "Compact Rifle",
  WEAPON_MACHINEPISTOL = "Machine Pistol",
  WEAPON_SWITCHBLADE = "Switchblade",
  WEAPON_REVOLVER = "Heavy Revolver",
  WEAPON_DOUBLEACTION = "Double-Action Revolver",
  WEAPON_MARKSMANPISTOL = "Marksman Pistol",
  WEAPON_RAYPISTOL = "Up-n-Atomizer",
  WEAPON_RAYCARBINE = "Unholy Hellbringer",
  WEAPON_RAYMINIGUN = "Widowmaker",
  WEAPON_NAVYREVOLVER = "Navy Revolver",
  WEAPON_CERAMICPISTOL = "Ceramic Pistol",
  WEAPON_STONE_HATCHET = "Stone Hatchet",
  WEAPON_PIPEWRENCH = "Pipe Wrench",
  WEAPON_PISTOL_MK2 = "Pistol Mk II",
  WEAPON_SNSPISTOL_MK2 = "SNS Pistol Mk II",
  WEAPON_REVOLVER_MK2 = "Heavy Revolver Mk II",
  WEAPON_SMG_MK2 = "SMG Mk II",
  WEAPON_PUMPSHOTGUN_MK2 = "Pump Shotgun Mk II",
  WEAPON_SPECIALCARBINE_MK2 = "Special Carbine Mk II",
  WEAPON_BULLPUPRIFLE_MK2 = "Bullpup Rifle Mk II",
  WEAPON_COMBATMG_MK2 = "Combat MG Mk II",
  WEAPON_HEAVYSNIPER_MK2 = "Heavy Sniper Mk II",
  WEAPON_ASSAULTRIFLE_MK2 = "Assault Rifle Mk II",
  WEAPON_CARBINERIFLE_MK2 = "Carbine Rifle Mk II",
  WEAPON_MILITARYRIFLE = "Military Rifle",
  WEAPON_COMBATSHOTGUN = "Combat Shotgun",
  WEAPON_GADGETPISTOL = "Perico Pistol",
  WEAPON_EMPLAUNCHER = "Compact EMP Launcher",
  WEAPON_HEAVYRIFLE = "Heavy Rifle",
  WEAPON_PRECISIONRIFLE = "Precision Rifle",
  WEAPON_TACTICALRIFLE = "Service Carbine"
}

local WeaponList = {}

for k,v in pairs(WeaponNames) do
    local hash = joaat(k)
    WeaponList[hash] = {label = v, value = k}
end

setmetatable(WeaponList, {
    __call = function(self, hash)
        if not hash then return end
        local value = self[hash]
        return value.label, value.value
    end
})

return WeaponList
```

