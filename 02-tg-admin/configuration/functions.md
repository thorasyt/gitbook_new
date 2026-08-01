# Shared Functions

The shared functions configuration for `tg-admin` manages localization retrieval, system identifier formats, custom notifications wrappers, and chat integrations. This is located in `shared/functions.lua` within the `tg-admin` resource directory.

---

### Locale Loader

#### getLocales
----
Retrieves translation strings dynamically from active locale file files.

##### Parameters:
- `section`: (string) Translation category category.
- `key`: (string) Optional. Target translation translation label.
- `element`: (string) Optional. Nested value inside translation objects.

##### Return:
- `string | table`: The translated text, or a fallback warning string (e.g. `"MISSING_KEY"`) if not found.

##### Example
```lua
local headerText = getLocales('menu', 'header_title')
```

---

### Menu / Event ID Formatter

#### CreateMenuId
----
Generates structured IDs for NUI lists or net event registrations to prevent conflicts.

##### Parameters:
- `type`: (string) Type of ID: `'menu'` | `'event'`.
- `id`: (string) Unique custom string name.

##### Return:
- `string`: Formatted identifier prefix.
  - If `'menu'`: returns `'tg-admin:menu:<id>'`
  - If `'event'`: returns `'tg-admin:event:<id>'`

##### Example
```lua
local menuId = CreateMenuId('menu', 'vehicle_customs')
```

---

### Notification Wrappers

These functions route alert notifications through `tg_lib` systems.

#### cl_notification
----
Displays a client-side warning toast message to the local player.

##### Parameters:
- `msg`: (string) Message content.
- `type`: (string) Notification type: `'success'` | `'error'` | `'warning'` | `'info'`.

##### Example
```lua
cl_notification("Action executed successfully", "success")
```

#### sr_notification
----
Triggers a server-side warning toast message on the target player's screen.

##### Parameters:
- `src`: (number) Target player's server ID.
- `msg`: (string) Message content.
- `type`: (string) Notification type: `'success'` | `'error'` | `'warning'` | `'info'`.

##### Example
```lua
sr_notification(targetPlayerId, "You have been healed", "success")
```

---

### Staff Chat Integration

#### sr_addChatMessage
----
Broadcasts staff messages using the active chat script.

##### Parameters:
- `name`: (string) Sender name.
- `message`: (string) Message text.

> [!NOTE]
> By default, if `Config.chatscript` is set to `'okokchat'`, this triggers `okokChat:ServerMessage` with a custom linear-gradient theme. You can edit this block to link your own chat system commands.

##### Example
```lua
sr_addChatMessage("Admin John", "Event is starting at the beach!")
```

---

### Ped Settings Hook

#### sr_pedmenusettings
----
Custom server-side hook executed when modifying/skinning a player ped.

##### Parameters:
- `target`: (number) Target player server ID.

##### Example
```lua
function sr_pedmenusettings(target)
    -- Add custom skin/ped persistence logic here if required
end
```
