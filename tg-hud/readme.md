# tg-hud

> **An ultra-modern, high-performance, and fully customizable HUD solution for FiveM.**
> Built with **Lua 5.4**, **React**, **TypeScript**, and **Vite**, powered by **`ox_lib`**, with native support for **ESX**, **Qbox (`qbx_core`)**, and **QBCore**.

---

## 🌟 Overview

**`tg-hud`** is a complete, feature-rich heads-up display system designed to provide roleplay servers with unparalleled visual aesthetics, fluid responsiveness, and deep player customization. Featuring **24 player status HUD styles**, **6 vehicle speedometer styles**, **5 server info styles**, custom **rounded & circular minimap streaming**, built-in **cruise control** and **seatbelt physics**, an advanced **stress system**, dynamic **weapon HUD**, and a full in-game **visual drag-and-drop editor** with RGBA color pickers.

Every client can customize their HUD layout, colors, and behavior on the fly, with their preferences stored persistently through FiveM KVP.

---

## ✨ Features at a Glance

### 1. 👤 Player Status HUD (24 Unique Styles)
Choose from 24 beautifully designed status indicator styles grouped into 3 distinct aesthetic categories:
- **Square (8 styles):**
  - Square Solid Card (`square_solid_card`)
  - Square Border Ring (`square_border_ring`)
  - Square Liquid Fill (`square_liquid_fill`)
  - Square Dashed (`square_dashed`)
  - Square Pie Fill (`square_pie_fill`)
  - Neon Arch (`neon_arch`)
  - Neon Tall Portal (`neon_tall_portal`)
  - Square Dual Split (`square_dual_split`)
- **Circle (8 styles):**
  - Circle Solid Disc (`circle_solid_disc`)
  - Circle Radial Progress (`circle_radial_progress`)
  - Circle Liquid Fill (`circle_liquid_fill`)
  - Circle Dashed Ring (`circle_dashed_ring`)
  - Circle Pie Fill (`circle_pie_fill`)
  - Circle Neon Arch (`circle_neon_arch`)
  - Neon Horseshoe Arc (`neon_horseshoe_arc`)
  - Circle Dual Split (`circle_dual_split`)
- **Minimal / Linear (8 styles):**
  - Full Labeled Bar (`full_labeled_bar`)
  - Squircle Badge & % (`squircle_badge_percent`)
  - Minimal Icon & % (`minimal_icon_percent`)
  - Capsule Pill Badge (`capsule_pill_badge`)
  - Capsule Slider Bar (`capsule_slider_bar`)
  - Minimal Linear Bar (`minimal_linear_bar`)
  - Underline Bar Stack (`underline_bar_stack`)
  - Vertical Pillar Bar (`vertical_pillar_bar`)

**Supported Status Vitals:**
- ❤️ **Health** (Accurate down/death state handling)
- 🛡️ **Armor**
- 🍔 **Hunger** (Auto-updated from ESX Status / QB / Qbox state bags)
- 💧 **Thirst** (Auto-updated from ESX Status / QB / Qbox state bags)
- ⚡ **Stamina & Oxygen** (Automatically detects underwater diving and switches to oxygen tracking)
- 🧠 **Stress** (Integrated with dynamic screen blur & ragdoll effects)
- 🎙️ **Voice Activity** (Real-time talk indicator & proximity range from `pma-voice`)

---

### 2. 🎨 In-Game RGBA Color Customizer
- Full color customization palette directly in the settings menu.
- Individually recolor any vitals bar (**Health, Armor, Hunger, Thirst, Stamina, Stress, Voice**).
- Dynamic CSS root variable updating with real-time preview.
- One-click restore default colors option.

---

### 3. 🚗 Vehicle HUD & Speedometer (6 Styles)
- **6 Modern Speedometer Designs:** Sleek visual tachometer dials, digital numbers, and gauges.
- **Multimodal Vehicle Support:**
  - 🏎️ **Land Vehicles:** Speedometer (KM/H or MP/H), RPM gauge, Gear position, Fuel gauge, Engine health/damage indicator, Headlights state (Off, Low Beam, High Beam), Cruise Control status, Seatbelt warning.
  - ✈️ **Air Vehicles (Planes & Helicopters):** Aviation instruments including Altimeter (height above ground), Pitch & Roll horizon indicators, Landing gear status, Engine health, Fuel.
  - 🛥️ **Water Vehicles (Boats & Jetskis):** Marine instruments including speed, Anchor status indicator, Engine health, Fuel.
- **Universal Fuel System Support:** Auto-detects and integrates with 13+ popular fuel scripts:
  - `LegacyFuel`, `ox_fuel`, `Renewed-Fuel`, `ps-fuel`, `lj-fuel`, `cdn-fuel`, `rcore_fuel`, `ti_fuel`, `lc_fuel`, `hyon_gas_station`, `okokGasStation`, `nd_fuel`, `myFuel`, or native GTA fuel fallback.
- **Built-in Cruise Control:**
  - Keybind controlled (Default: `J`).
  - Automatically holds forward speed on all 4 wheels.
  - Safely cancels upon braking, drifting, engine cutoff, or vehicle exit.
  - In-flight speed adjustment (tap cruise key to adjust).
- **Realistic Seatbelt & Windscreen Ejection:**
  - Keybind controlled (Default: `B`).
  - High-speed front impact collision detection calculated against vehicle velocity vector.
  - Unbuckled players are ejected through smashed windscreens with ragdoll physics and impact damage.
  - Optional config setting to lock vehicle exit while buckled (`ExitOnSeatBelt = false`).
  - Configurable emergency vehicle exemption and passenger rules.

---

### 4. 🧭 Minimap / Radar & Compass Navigation
- **Custom Streamed Radar Textures:**
  - **Rounded** radar mask (`tgmap_round.ytd`, `minimap.gfx`).
  - **Circular** radar mask (`tgmap.ytd`, `minimap.gfx`).
- **Visibility Options:**
  - `Always`: Radar is visible on foot and in vehicle.
  - `In Vehicle`: Radar only displays when seated inside a vehicle.
- **Interactive Radar Repositioning:**
  - Move and scale the GTA radar interactively through the in-game UI editor.
  - Pre-calibrated aspect ratio alignment from 800x600 up to 4K resolutions.
- **Live Location & Compass Bar:**
  - Real-time **Street Name** and **Zone / District** tracking.
  - 8-point **Cardinal Direction** readout (`N`, `NW`, `W`, `SW`, `S`, `SE`, `E`, `NE`).
  - **Tracking Behavior:** Tracks **Player Movement** heading or **Mouse / Camera** orientation.
  - **Clock Mode:** Display **In-Game Time** or **Real-World Local Time**.

---

### 5. 💳 Server & Character Info HUD (5 Styles)
- **5 Clean Layout Options** positioned on the HUD.
- **Information Tracked:**
  - Server ID / OP number
  - Character Full Name
  - Cash on hand (live synchronization)
  - Bank balance (live synchronization)
  - Job title and Grade name
  - Gang title and Grade name (QBCore / Qbox)
  - Current Voice Range (Whisper, Normal, Shout)

---

### 6. 🔫 Dynamic Weapon HUD
- Automatically slides out when equipping a firearm or melee weapon.
- Real-time weapon icon displays with custom `.webp` renders for 60+ GTA V and DLC weapons.
- Vehicle mounted weapon detection (e.g., weaponized vehicles).
- Smart ammo display:
  - Clip-based weapons: `[Current Clip] / [Reserve Ammo]` (e.g., `30/120`).
  - Non-clip weapons / Melee / Throwables: Total ammo count.
- Native GTA weapon and ammo HUD components cleanly hidden.

---

### 7. 🧠 Advanced Stress System
- **Realistic Stress Acquisition:**
  - High-speed driving without a seatbelt (and extreme speeds with a seatbelt).
  - Vehicle class filtering (sports, supercars, muscles, etc.).
  - Weapon discharge: Firing weapons adds stress based on configurable probability.
  - Whitelist protections: Emergency services / police jobs, non-lethal/utility equipment (fire extinguisher, petrol can, hazard can).
- **Dynamic Stress Effects:**
  - Tiered screen blur scaling between stress level 50 and 99.
  - Severe symptoms at 100 stress: heavy persistent blur, screen blackout pulses, and stumbling/ragdoll falls.
- Full client-side exports and server-side state bags for external script integration (drugs, alcohol, smoking, meditation).

---

### 8. 🛠️ In-Game Customization Menu (`/edithud`)
- **Live Drag-and-Drop:** Reposition Status HUD, Minimap, Speedometer, Info HUD, Compass, and Weapon HUD to any pixel coordinates.
- **Style Selector:** Switch between HUD styles, Speedometer layouts, and Info cards instantly.
- **Basic Configuration:**
  - Toggle Minimap shape (Rounded / Circular).
  - Toggle Minimap display (Always / Vehicle Only).
  - Toggle Compass heading source (Player / Camera).
  - Toggle Time mode (Game Time / Real Time).
  - Toggle Speed units (KM/H / MP/H).
  - Performance profile selector (Ultra, Balanced, Smooth, Low).
- **Cinematic Mode:** Toggle black letterbox widescreen bars and hide all HUD elements for cinematic roleplay clips and photography.
- **Persistent Storage:** Client settings are saved locally to KVP (`tg_hud:data`), avoiding server SQL queries and maintaining preferences across sessions.

---

### 9. ⚡ Optimization & Compatibility
- **Performance Profiles:**
  - `Ultra`: 100-150ms refresh loops.
  - `Balanced`: 250-300ms refresh loops.
  - `Smooth`: 450-500ms refresh loops.
  - `Low`: 600-700ms refresh loops.
- **Framework Auto-Detection:** Works automatically with ESX, QBCore, and Qbox.
- **ox_inventory Integration:** Automatically hides HUD elements while the inventory screen is open.
- **Pause Menu Handling:** Hides the HUD whenever the GTA pause menu is active.
- Powered by `ox_lib` caching (`cache.ped`, `cache.vehicle`, `cache.weapon`, `cache.playerId`).

---

## 📋 Requirements & Dependencies

| Resource | Required | Description |
|---|---|---|
| [`ox_lib`](https://github.com/overextended/ox_lib) | **Yes** | Shared utility library, keybinds, caching, and commands |
| Framework (`es_extended`, `qbx_core`, or `qb-core`) | **Yes** | Player data, money, jobs, and needs |
| [`pma-voice`](https://github.com/AvarianKnight/pma-voice) | Optional | Voice range proximity and talking indicators |
| [`ox_inventory`](https://github.com/overextended/ox_inventory) | Optional | Auto-hiding HUD on inventory open |
| Any Supported Fuel Resource | Optional | Vehicle fuel levels (LegacyFuel, ox_fuel, cdn-fuel, etc.) |

---
