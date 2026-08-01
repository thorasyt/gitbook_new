# Shared Configuration

The shared configuration for `tg-admin` controls language settings, admin overlay distances, item exemptions, and custom presets. This is located in `shared/config.lua` within the `tg-admin` resource directory.

---

### Configuration Parameters

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `Config.locales` | `string` | `'en'` | Active localization language code. |
| `Config.DefaultNpcDensityController` | `boolean` | `false` | Default status of the NPC traffic and pedestrian density controller. |
| `Config.TagDisplayDistance` | `number` | `150` | Maximum distance (in meters) to render overhead player name tags. |
| `Config.BlipDisplayDistance` | `number` | `250` | Maximum distance (in meters) to display player blips on the map/radar. |
| `Config.chatscript` | `string` | `'okokchat'` | The chat resource integration name (e.g., `'okokchat'`). |
| `Config.keepItemInInventory` | `array` | `{ 'weapon_carbinerifle' }` | List of items that are exempted from being removed during inventory wipes. |

---

### Custom Vehicle Customizer Options

These arrays specify color presets, headlight settings, and vehicle customizer mods exposed in the Admin Vehicle Mod menu.

#### 1. Underglow Neon Colors (`Config.Mod.neon`)
Configures preset color mixtures (RGB values) for neon lighting:
```lua
Config.Mod.neon = {
    {label = 'White',         value = {222, 222, 255}},
    {label = 'Blue',          value = {2, 21, 255}},
    {label = 'Electric Blue', value = {3, 83, 255}},
    {label = 'Mint Green',    value = {0, 255, 140}},
    -- ...
}
```

#### 2. Tyre Smoke Colors (`Config.Mod.tyreColor`)
Configures custom tyre burnout smoke presets:
```lua
Config.Mod.tyreColor = {
    {label = 'White Smoke',  r = 254, g = 254, b = 254},
    {label = 'Black Smoke',  r = 1,   g = 1,   b = 1},
    {label = 'Blue Smoke',   r = 0,   g = 150, b = 255},
    -- ...
}
```

#### 3. Xenon Lights Headlight Colors (`Config.XenonColor`)
Maps custom headlight style names to their native GTA Xenon color indexes:
```lua
Config.XenonColor = {
    ['Default'] = -1,
    ['White'] = 0,
    ['Blue'] = 1,
    ['Electric Blue'] = 2,
    -- ...
}
```

---

### Custom Paint Options

Maps paint options to their GTA V texture mod indexes.

#### 1. Chameleon Paint Colors (`Config.Chameleon`)
```lua
Config.Chameleon = {
    ['Anodized Red Pearl'] = 161,
    ['Anodized Wine Pearl'] = 162,
    ['Anodized Purple Pearl'] = 163,
    -- ...
}
```

#### 2. Metallic Colors (`Config.Metallic`)
```lua
Config.Metallic = {
    ['Black'] = 0,
    ['Graphite'] = 1,
    ['Black Steel'] = 2,
    -- ...
}
```

#### 3. Matte Colors (`Config.Matte`)
```lua
Config.Matte = {
    ['Black'] = 12,
    ['Gray'] = 13,
    ['Light Gray'] = 14,
    -- ...
}
```

#### 4. Metal Styles (`Config.Metals`)
```lua
Config.Metals = {
    ['Brushed Steel'] = 117,
    ['Brushed Black Steel'] = 118,
    ['Brushed Aluminum'] = 119,
    -- ...
}
```

#### 5. Chrome (`Config.Crome`)
```lua
Config.Crome = {
    ['Chrome'] = 120
}
```

---

### Ped Model Spawning Catalog (`Config.Peds`)

Contains standard models registered in the player customization skin changer menu, grouped by categories (e.g. `Ambient female`, `Ambient male`, `Animals`, `Cutscene`, `Gang male`, `Story`).

##### Example
```lua
Config.Peds = {
    {item = 'a_f_m_beach_01', category = 'Ambient female'},
    {item = 'a_m_m_acult_01', category = 'Ambient male'},
    {item = 'a_c_boar', category = 'Animals'},
    -- ...
}
```
