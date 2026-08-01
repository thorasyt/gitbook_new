# Server Configuration

The server-side configuration for `tg-admin` manages server-side feature permissions, administrative chat commands, and audit logging triggers. This is located in `shared/server.lua` within the `tg-admin` resource directory.

---

### Administrative Permissions

Ranks assigned to each administrative action. Only users belonging to the configured rank (or higher) will be allowed to execute them.

```lua
ser_cfg.Permissions = {
    ['mainmenu']      = 'admin',
    ['playerstate']   = 'admin',
    ['serveroption']  = 'admin',
    ['healer']        = 'admin',
    ['teleport']      = 'admin',
    ['sendmessage']   = 'admin',
    ['skincard']      = 'admin',
    ['addmoney']      = 'admin',
    ['setjob']        = 'admin',
    ['setgang']        = 'admin',
    ['itemmanage']    = 'admin',
    ['relog']         = 'admin',
    ['spawnvehicle']  = 'admin',
    ['givevehicle']   = 'admin',
    ['kick']          = 'admin',
    ['ban']           = 'admin',
    ['cleararea']     = 'admin',
    ['announcement']  = 'admin',
    ['reviveoption']  = 'admin',
    ['troll_server']  = 'admin',
    ['clearinv']      = 'admin',
    ['setped']        = 'admin',
    ['resource']      = 'admin',
    ['jobmanage']     = 'admin'
}
```

---

### Registered Chat Commands

These are shorthand chat commands registered on the server. Admins can execute these commands in the chat box to trigger fast actions.

| Command Key | Default Chat Trigger | Description | Parameters |
| :--- | :--- | :--- | :--- |
| `mainpannel` | `/tg [targetId]` | Opens the NUI Admin Panel dashboard. | `target`: (optional) Player server ID. |
| `revive` | `/rv <targetId>` | Revives a player. | `target`: Player server ID. |
| `goto` | `/gt <targetId>` | Teleports you to the player. | `target`: Player server ID. |
| `bring` | `/br <targetId>` | Teleports the player to you. | `target`: Player server ID. |
| `addmoney` | `/am <targetId> <account> <amount>` | Gives money to the player. | `target`: ID, `account`: cash/bank, `amount`: value. |
| `setjob` | `/sj <targetId> <job> <grade>` | Sets the player's job. | `target`: ID, `job`: name, `grade`: grade level. |
| `additem` | `/gi <targetId> <item> <count>` | Gives an item to the player. | `target`: ID, `item`: name, `count`: amount. |
| `propmenu` | `/pmenu` | Opens the prop spawning placement overlay. | None. |
| `trollmenu` | `/troll [targetId]` | Opens the troll actions panel overlay. | `target`: (optional) Player server ID. |

---

### Action Logging Integration

Whenever an admin executes a command (e.g. banning, reviving, clearing areas, giving items), the server triggers this logging function. You can integrate your own Discord webhooks or logging frameworks here.

```lua
ser_cfg.sendLogs = function(src, target, action)
    -- Default log trigger (console logs)
    TriggerEvent('tg-admin:server:consolelogs', src, target, action)
    
    -- Add your custom Discord webhook or logging script execution below
end
```
