# Client Configuration

The client configuration for `tg-admin` manages default key mappings and client-side administrative feature permissions. This is typically located in `config/client.lua` (or similar client config files) within the `tg-admin` resource directory.

---

### Configuration Block

```lua
return {
    keybinds = {
        ['mainmenu'] = 'f9',
        ['pannelkey'] = 'o'
    },
    permissions = {
        ['viewself'] = 'admin',
        ['serveroption'] = 'admin',
        ['showcoords'] = 'admin',
        ['showplayername'] = 'admin',
        ['showplayerblip'] = 'admin',
        ['tpcoords'] = 'admin',
        ['troll_client'] = 'admin'
    } 
}
```

---

### Keybinds Settings

Define the default hotkeys mapped for launching the administration interfaces. Players can also rebind these keys in their GTA V/FiveM pause menu settings under **Key Bindings > FiveM**.

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `mainmenu` | `string` | `'f9'` | Hotkey mapped to toggle open the main administration client-side menu. |
| `pannelkey` | `string` | `'o'` | Hotkey mapped to toggle open the main React-based NUI Admin Panel dashboard. |

---

### Permissions settings

Configure the minimum administrative group rank (defined via framework groups or ACE permissions) required to execute specific client-side features.

| Parameter | Type | Required Rank | Description |
| :--- | :--- | :--- | :--- |
| `viewself` | `string` | `'admin'` | Permission required to inspect your own character details card. |
| `serveroption` | `string` | `'admin'` | Permission required to access global server parameters (time/weather adjustments, entity density). |
| `showcoords` | `string` | `'admin'` | Permission required to toggle the HUD coordinate helper overlay. |
| `showplayername` | `string` | `'admin'` | Permission required to view player overhead name tags. |
| `showplayerblip` | `string` | `'admin'` | Permission required to toggle player location blips on the map/radar. |
| `tpcoords` | `string` | `'admin'` | Permission required to teleport to specific map coordinates. |
| `troll_client` | `string` | `'admin'` | Permission required to execute interactive player actions (freeze, burn, slap). |
