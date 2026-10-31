# shared/defaultsize.lua

This document provides a comprehensive breakdown of **`shared/defaultsize.lua`**. It details the responsive screen resolution matrix, default HUD component coordinates, scaling parameters, internal lookup logic, and guidelines for adding custom screen resolutions.

---

## 📑 Table of Contents

1. [Overview & Metatable Pattern](#1-overview--metatable-pattern)
2. [HUD Component Properties](#2-hud-component-properties)
   - [Compass (`compass`)](#compass-compass)
   - [Minimap / Radar (`radar`)](#minimap--radar-radar)
   - [Status HUD (`hud`)](#status-hud-hud)
   - [Weapon HUD (`weapon`)](#weapon-hud-weapon)
   - [Server Info (`info`)](#server-info-info)
   - [Speedometer / Vehicle HUD (`vehicle`)](#speedometer--vehicle-hud-vehicle)
3. [Supported Resolution Profiles](#3-supported-resolution-profiles)
   - [4:3 & 5:4 Standard Aspect Ratios](#43--54-standard-aspect-ratios)
   - [16:10 Widescreen Aspect Ratios](#1610-widescreen-aspect-ratios)
   - [16:9 Standard High Definition (HD / FHD)](#169-standard-high-definition-hd--fhd)
   - [1440p (2K QHD) & 2160p (4K UHD) Ultra-High Definition](#1440p-2k-qhd--2160p-4k-uhd-ultra-high-definition)
4. [Resolution Matrix Comparison Table](#4-resolution-matrix-comparison-table)
5. [Client Integration & Reset Logic](#5-client-integration--reset-logic)
6. [Adding Custom Resolutions (e.g., Ultrawide 21:9)](#6-adding-custom-resolutions-eg-ultrawide-219)
7. [Full Configuration Source Code](#7-full-configuration-source-code)

---

## 1. Overview & Metatable Pattern

The `resolutionSize` configuration table defines the baseline pixel positions (`x`, `y`) and dimensions (`size`, `width`, `height`) for all six core HUD widgets across 13 common display resolutions.

At the bottom of `shared/defaultsize.lua`, the table is wrapped with a metatable:

```lua
setmetatable(resolutionSize, {
    __call = function(self, name)
        if name == nil then return nil end
        return self[name]
    end
})

return resolutionSize
```

### Why this exists:
This metatable implements dual lookup syntax:
- **Function call syntax:** `resolutionSize('1920x1080')`
- **Table key index syntax:** `resolutionSize['1920x1080']`

If an invalid or `nil` resolution key is supplied, it safely returns `nil` without throwing runtime errors.

---

## 2. HUD Component Properties

Each resolution key maps to an object containing configuration profiles for the six core interface widgets:

```
┌─────────────────────────────────────────────────────────────┐
│ [info]                                                      │
│                                                             │
│                                                             │
│                                            [weapon]         │
│                                                             │
│ [radar]                                    [vehicle]        │
│ [compass]                                  [hud]            │
└─────────────────────────────────────────────────────────────┘
```

### Compass (`compass`)
Controls the navigation heading ribbon displayed at the bottom-left of the screen.
- **`x`** (`number`): Horizontal pixel offset from the left edge of the screen.
- **`y`** (`number`): Vertical pixel offset from the top edge of the screen.
- **`size`** (`number`): Total width / scale multiplier for the heading ribbon.

### Minimap / Radar (`radar`)
Controls the container size and position of the GTA V minimap / radar frame.
- **`x`** (`number`): Horizontal pixel position of the minimap viewport.
- **`y`** (`number`): Vertical pixel position of the minimap viewport.
- **`width`** (`number`): Pixel width of the radar viewport mask.
- **`height`** (`number`): Pixel height of the radar viewport mask.
- **`size`** (`number`): Bounding scale factor for the radar border container.

### Status HUD (`hud`)
Controls the main player vital status cluster (Health, Armor, Hunger, Thirst, Stress, Oxygen, Voice/Stamina).
- **`x`** (`number`): Horizontal pixel position of the status cluster.
- **`y`** (`number`): Vertical pixel position of the status cluster.
- **`size`** (`number`): Uniform scale size (in pixels) for the individual status indicators/circles/bars.

### Weapon HUD (`weapon`)
Controls the active weapon card, crosshair mode, and ammunition / magazine counter.
- **`x`** (`number`): Horizontal pixel position of the weapon HUD card.
- **`y`** (`number`): Vertical pixel position of the weapon HUD card.
- **`size`** (`number`): Width / scaling factor for the weapon graphic and ammo counter.

### Server Info (`info`)
Controls the server identity card showing Server Branding/Logo, Player ID, Cash, Bank balance, Job title, and Gang affiliation.
- **`x`** (`number`): Horizontal pixel position of the info card.
- **`y`** (`number`): Vertical pixel position of the info card (typically anchored near the top-right).
- **`size`** (`number`): Scale size / bounding width for the info card.

### Speedometer / Vehicle HUD (`vehicle`)
Controls the vehicle telemetry cluster (Speed digits, Unit KM/H or MPH, Gear, Fuel gauge, Engine health, Seatbelt indicator, Cruise control, Lights, and Nitrous).
- **`x`** (`number`): Horizontal pixel position of the speedometer cluster.
- **`y`** (`number`): Vertical pixel position of the speedometer cluster.
- **`size`** (`number`): Scale size / dial radius multiplier for the speedometer widget.

---

## 3. Supported Resolution Profiles

### 4:3 & 5:4 Standard Aspect Ratios
Optimized for legacy monitors and competitive stretch-resolution setups.

#### `800x600` (4:3 SVGA)
```lua
['800x600'] = {
    compass = { x = 12, y = 550, size = 180 },
    radar   = { x = 9,  y = 407, width = 200, height = 130, size = 200 },
    hud     = { x = 630, y = 560, size = 25 },
    weapon  = { x = 788, y = 300, size = 68 },
    info    = { x = 788, y = 12,  size = 50 },
    vehicle = { x = 788, y = 535, size = 40 },
}
```

#### `1024x768` (4:3 XGA)
```lua
['1024x768'] = {
    compass = { x = 16, y = 710, size = 220 },
    radar   = { x = 16, y = 535, width = 240, height = 145, size = 240 },
    hud     = { x = 1008, y = 720, size = 36 },
    weapon  = { x = 1008, y = 384, size = 78 },
    info    = { x = 1008, y = 16,  size = 65 },
    vehicle = { x = 1008, y = 690, size = 80 },
}
```

#### `1280x1024` (5:4 SXGA)
```lua
['1280x1024'] = {
    compass = { x = 18, y = 960, size = 260 },
    radar   = { x = 18, y = 765, width = 280, height = 165, size = 280 },
    hud     = { x = 1262, y = 965, size = 42 },
    weapon  = { x = 1262, y = 512, size = 88 },
    info    = { x = 1262, y = 18,  size = 85 },
    vehicle = { x = 1262, y = 930, size = 88 },
}
```

---

### 16:10 Widescreen Aspect Ratios
Common on business displays, Apple Mac displays, and older widescreen monitors.

#### `1440x900` (16:10 WXGA+)
```lua
['1440x900'] = {
    compass = { x = 18, y = 835, size = 260 },
    radar   = { x = 18, y = 645, width = 270, height = 155, size = 270 },
    hud     = { x = 1422, y = 845, size = 42 },
    weapon  = { x = 1422, y = 450, size = 86 },
    info    = { x = 1422, y = 18,  size = 80 },
    vehicle = { x = 1422, y = 810, size = 86 },
}
```

#### `1680x1050` (16:10 WSXGA+)
```lua
['1680x1050'] = {
    compass = { x = 20, y = 980, size = 280 },
    radar   = { x = 20, y = 785, width = 290, height = 165, size = 290 },
    hud     = { x = 1660, y = 990, size = 46 },
    weapon  = { x = 1660, y = 525, size = 92 },
    info    = { x = 1660, y = 20,  size = 90 },
    vehicle = { x = 1660, y = 950, size = 92 },
}
```

---

### 16:9 Standard High Definition (HD / FHD)
Standard gaming monitor and laptop display resolutions.

#### `1280x720` (16:9 HD / 720p)
```lua
['1280x720'] = {
    compass = { x = 16, y = 665, size = 240 },
    radar   = { x = 16, y = 495, width = 250, height = 145, size = 250 },
    hud     = { x = 1264, y = 675, size = 38 },
    weapon  = { x = 1264, y = 360, size = 80 },
    info    = { x = 1264, y = 16,  size = 70 },
    vehicle = { x = 1264, y = 645, size = 80 },
}
```

#### `1366x768` (16:9 WXGA Laptop Standard)
```lua
['1366x768'] = {
    compass = { x = 18, y = 710, size = 245 },
    radar   = { x = 18, y = 535, width = 260, height = 150, size = 260 },
    hud     = { x = 1348, y = 720, size = 40 },
    weapon  = { x = 1348, y = 384, size = 82 },
    info    = { x = 1348, y = 18,  size = 75 },
    vehicle = { x = 1348, y = 688, size = 82 },
}
```

#### `1600x900` (16:9 HD+)
```lua
['1600x900'] = {
    compass = { x = 18, y = 835, size = 270 },
    radar   = { x = 18, y = 640, width = 280, height = 160, size = 280 },
    hud     = { x = 1582, y = 845, size = 44 },
    weapon  = { x = 1582, y = 450, size = 88 },
    info    = { x = 1582, y = 18,  size = 85 },
    vehicle = { x = 1582, y = 810, size = 88 },
}
```

#### `1920x1080` (16:9 Full HD / 1080p - Primary Standard)
```lua
['1920x1080'] = {
    compass = { x = 20, y = 1010, size = 300 },
    radar   = { x = 20, y = 815,  width = 300, height = 165, size = 300 },
    hud     = { x = 1900, y = 1020, size = 48 },
    weapon  = { x = 1900, y = 540,  size = 96 },
    info    = { x = 1900, y = 20,   size = 100 },
    vehicle = { x = 1900, y = 980,  size = 100 },
}
```

---

### 1440p (2K QHD) & 2160p (4K UHD) Ultra-High Definition

#### `2560x1440` (16:9 2K QHD)
```lua
['2560x1440'] = {
    compass = { x = 26, y = 1350, size = 400 },
    radar   = { x = 26, y = 1090, width = 400, height = 220, size = 400 },
    hud     = { x = 2534, y = 1360, size = 64 },
    weapon  = { x = 2534, y = 720,  size = 128 },
    info    = { x = 2534, y = 26,   size = 133 },
    vehicle = { x = 2534, y = 1305, size = 133 },
}
```

#### `3840x2160` (16:9 4K UHD)
```lua
['3840x2160'] = {
    compass = { x = 40, y = 2025, size = 600 },
    radar   = { x = 40, y = 1635, width = 600, height = 330, size = 600 },
    hud     = { x = 3800, y = 2040, size = 96 },
    weapon  = { x = 3800, y = 1080, size = 192 },
    info    = { x = 3800, y = 40,   size = 200 },
    vehicle = { x = 3800, y = 1960, size = 200 },
}
```

---

## 4. Resolution Matrix Comparison Table

| Resolution | Aspect Ratio | Compass (X, Y / Size) | Radar (W x H / Size) | Status HUD (X, Y / Size) | Vehicle HUD (X, Y / Size) | Weapon HUD (X, Y / Size) | Info Card (X, Y / Size) |
|---|---|---|---|---|---|---|---|
| **800x600** | 4:3 | 12, 550 / 180 | 200x130 / 200 | 630, 560 / 25 | 788, 535 / 40 | 788, 300 / 68 | 788, 12 / 50 |
| **1024x768** | 4:3 | 16, 710 / 220 | 240x145 / 240 | 1008, 720 / 36 | 1008, 690 / 80 | 1008, 384 / 78 | 1008, 16 / 65 |
| **1128x634** | ~16:9 | 16, 585 / 210 | 230x135 / 230 | 1112, 590 / 34 | 1112, 565 / 75 | 1112, 317 / 74 | 1112, 14 / 60 |
| **1280x720** | 16:9 | 16, 665 / 240 | 250x145 / 250 | 1264, 675 / 38 | 1264, 645 / 80 | 1264, 360 / 80 | 1264, 16 / 70 |
| **1280x1024** | 5:4 | 18, 960 / 260 | 280x165 / 280 | 1262, 965 / 42 | 1262, 930 / 88 | 1262, 512 / 88 | 1262, 18 / 85 |
| **1366x768** | 16:9 | 18, 710 / 245 | 260x150 / 260 | 1348, 720 / 40 | 1348, 688 / 82 | 1348, 384 / 82 | 1348, 18 / 75 |
| **1440x900** | 16:10 | 18, 835 / 260 | 270x155 / 270 | 1422, 845 / 42 | 1422, 810 / 86 | 1422, 450 / 86 | 1422, 18 / 80 |
| **1600x900** | 16:9 | 18, 835 / 270 | 280x160 / 280 | 1582, 845 / 44 | 1582, 810 / 88 | 1582, 450 / 88 | 1582, 18 / 85 |
| **1680x1050** | 16:10 | 20, 980 / 280 | 290x165 / 290 | 1660, 990 / 46 | 1660, 950 / 92 | 1660, 525 / 92 | 1660, 20 / 90 |
| **1760x990** | 16:9 | 20, 925 / 285 | 290x160 / 290 | 1740, 935 / 46 | 1740, 895 / 92 | 1740, 495 / 92 | 1740, 20 / 92 |
| **1920x1080** | 16:9 | 20, 1010 / 300 | 300x165 / 300 | 1900, 1020 / 48 | 1900, 980 / 100 | 1900, 540 / 96 | 1900, 20 / 100 |
| **2560x1440** | 16:9 | 26, 1350 / 400 | 400x220 / 400 | 2534, 1360 / 64 | 2534, 1305 / 133 | 2534, 720 / 128 | 2534, 26 / 133 |
| **3840x2160** | 16:9 | 40, 2025 / 600 | 600x330 / 600 | 3800, 2040 / 96 | 3800, 1960 / 200 | 3800, 1080 / 192 | 3800, 40 / 200 |

---

## 5. Client Integration & Reset Logic

### How the Client Resolves Layouts
1. **Resolution Detection:** On client startup or display mode switch, the client queries FiveM native:
   ```lua
   local screenW, screenH = GetActiveScreenResolution()
   local resKey = string.format('%dx%d', screenW, screenH)
   ```
2. **KVP Check:** The client checks for locally stored layout overrides under KVP key `tg_hud:data`.
3. **Fallback to `defaultsize.lua`:** If no KVP exists (first join) or if the player executes the **"Reset to Default"** action inside `/edithud`, the client loads `resolutionSize(resKey)`.
4. **Safety Fallback:** If a player uses an unsupported custom resolution, the system safely falls back to the nearest standard 16:9 profile (`1920x1080`).

---

## 6. Adding Custom Resolutions (e.g., Ultrawide 21:9)

To add support for 21:9 Ultrawide (such as `2560x1080` or `3440x1440`) or 32:9 Super Ultrawide (`5120x1440`), append a new key to the `resolutionSize` table in `shared/defaultsize.lua`:

```lua
['2560x1080'] = {
    compass = { x = 20, y = 1010, size = 300 },
    radar   = { x = 20, y = 815,  width = 300, height = 165, size = 300 },
    hud     = { x = 2540, y = 1020, size = 48 },
    weapon  = { x = 2540, y = 540,  size = 96 },
    info    = { x = 2540, y = 20,   size = 100 },
    vehicle = { x = 2540, y = 980,  size = 100 },
},
```

> [!TIP]
> Keep the `radar` and `compass` anchored to the lower-left (`x` between `16` and `30`), while anchoring `hud`, `weapon`, `info`, and `vehicle` relative to the right edge (`x ≈ screenWidth - 20`).

---

## 7. Full Configuration Source Code

```lua
local resolutionSize = {
    ['800x600'] = {
        compass = { x = 12, y = 550, size = 180 },
        radar = { x = 9, y = 407, width = 200, height = 130, size = 200 },
        hud = { x = 630, y = 560, size = 25 },
        weapon = { x = 788, y = 300, size = 68 },
        info = { x = 788, y = 12, size = 50 },
        vehicle = { x = 788, y = 535, size = 40 },
    },    
    ['1024x768'] = {
        compass = { x = 16, y = 710, size = 220 },
        radar = { x = 16, y = 535, width = 240, height = 145, size = 240 },
        hud = { x = 1008, y = 720, size = 36 },
        weapon = { x = 1008, y = 384, size = 78 },
        info = { x = 1008, y = 16, size = 65 },
        vehicle = { x = 1008, y = 690, size = 80 },
    },    
    ['1128x634'] = {
        compass = { x = 16, y = 585, size = 210 },
        radar = { x = 16, y = 430, width = 230, height = 135, size = 230 },
        hud = { x = 1112, y = 590, size = 34 },
        weapon = { x = 1112, y = 317, size = 74 },
        info = { x = 1112, y = 14, size = 60 },
        vehicle = { x = 1112, y = 565, size = 75 },
    },    
    ['1280x720'] = {
        compass = { x = 16, y = 665, size = 240 },
        radar = { x = 16, y = 495, width = 250, height = 145, size = 250 },
        hud = { x = 1264, y = 675, size = 38 },
        weapon = { x = 1264, y = 360, size = 80 },
        info = { x = 1264, y = 16, size = 70 },
        vehicle = { x = 1264, y = 645, size = 80 },
    },    
    ['1280x1024'] = {
        compass = { x = 18, y = 960, size = 260 },
        radar = { x = 18, y = 765, width = 280, height = 165, size = 280 },
        hud = { x = 1262, y = 965, size = 42 },
        weapon = { x = 1262, y = 512, size = 88 },
        info = { x = 1262, y = 18, size = 85 },
        vehicle = { x = 1262, y = 930, size = 88 },
    },    
    ['1366x768'] = {
        compass = { x = 18, y = 710, size = 245 },
        radar = { x = 18, y = 535, width = 260, height = 150, size = 260 },
        hud = { x = 1348, y = 720, size = 40 },
        weapon = { x = 1348, y = 384, size = 82 },
        info = { x = 1348, y = 18, size = 75 },
        vehicle = { x = 1348, y = 688, size = 82 },
    },    
    ['1440x900'] = {
        compass = { x = 18, y = 835, size = 260 },
        radar = { x = 18, y = 645, width = 270, height = 155, size = 270 },
        hud = { x = 1422, y = 845, size = 42 },
        weapon = { x = 1422, y = 450, size = 86 },
        info = { x = 1422, y = 18, size = 80 },
        vehicle = { x = 1422, y = 810, size = 86 },
    },    
    ['1600x900'] = {
        compass = { x = 18, y = 835, size = 270 },
        radar = { x = 18, y = 640, width = 280, height = 160, size = 280 },
        hud = { x = 1582, y = 845, size = 44 },
        weapon = { x = 1582, y = 450, size = 88 },
        info = { x = 1582, y = 18, size = 85 },
        vehicle = { x = 1582, y = 810, size = 88 },
    },    
    ['1680x1050'] = {
        compass = { x = 20, y = 980, size = 280 },
        radar = { x = 20, y = 785, width = 290, height = 165, size = 290 },
        hud = { x = 1660, y = 990, size = 46 },
        weapon = { x = 1660, y = 525, size = 92 },
        info = { x = 1660, y = 20, size = 90 },
        vehicle = { x = 1660, y = 950, size = 92 },
    },    
    ['1760x990'] = {
        compass = { x = 20, y = 925, size = 285 },
        radar = { x = 20, y = 735, width = 290, height = 160, size = 290 },
        hud = { x = 1740, y = 935, size = 46 },
        weapon = { x = 1740, y = 495, size = 92 },
        info = { x = 1740, y = 20, size = 92 },
        vehicle = { x = 1740, y = 895, size = 92 },
    },    
    ['1920x1080'] = {
        compass = { x = 20, y = 1010, size = 300 },
        radar = { x = 20, y = 815, width = 300, height = 165, size = 300 },
        hud = { x = 1900, y = 1020, size = 48 },
        weapon = { x = 1900, y = 540, size = 96 },
        info = { x = 1900, y = 20, size = 100 },
        vehicle = { x = 1900, y = 980, size = 100 },
    },    
    ['2560x1440'] = {
        compass = { x = 26, y = 1350, size = 400 },
        radar = { x = 26, y = 1090, width = 400, height = 220, size = 400 },
        hud = { x = 2534, y = 1360, size = 64 },
        weapon = { x = 2534, y = 720, size = 128 },
        info = { x = 2534, y = 26, size = 133 },
        vehicle = { x = 2534, y = 1305, size = 133 },
    },    
    ['3840x2160'] = {
        compass = { x = 40, y = 2025, size = 600 },
        radar = { x = 40, y = 1635, width = 600, height = 330, size = 600 },
        hud = { x = 3800, y = 2040, size = 96 },
        weapon = { x = 3800, y = 1080, size = 192 },
        info = { x = 3800, y = 40, size = 200 },
        vehicle = { x = 3800, y = 1960, size = 200 },
    },
}

setmetatable(resolutionSize, {
    __call = function(self, name)
        if name == nil then return nil end
        return self[name]
    end
})

return resolutionSize
```

