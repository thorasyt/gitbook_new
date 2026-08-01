# Server Side
You can easily call all of these functions inside your resource script by doing `TG.notification`.

### TG.notification.Show
----
Triggers a toast notification on the client screen.

##### Parameters:
- `src`: (number) Target player's server ID.
- `data`: (table) Config properties:
  - `title`: (string) Optional. Header title.
  - `message`: (string) Body description text message.
  - `type`: (string) Type: `'success'` | `'error'` | `'warning'` | `'info'` | `'police'` | `'admin'` | `'custom'` (default `'info'`).
  - `duration`: (number) Duration in milliseconds (default `5000`).
  - `style`: (number) Style index layout `1` | `2` | `3` | `4` | `5` | `6` (default `1`).
  - `icon`: (string) Optional. Lucide icon name.
  - `accentColor`: (string) Optional. Hex color string.

##### Return:
- `string`: Unique notification ID.

##### Example
```lua
TG.notification.Show(source, {
    title = 'Database Update',
    message = 'Your character data has been saved successfully.',
    type = 'success'
})
```


### TG.notification.Remove
----
Hides a notification before its timer expires.

##### Parameters:
- `src`: (number) Target player ID.
- `id`: (string) Notification unique ID.

##### Example
```lua
TG.notification.Remove(source, notificationId)
```


### Shorthand Helpers
The server offers standard helper channels matching the message category.
- `TG.notification.Success(src, message, title, duration, style)`
- `TG.notification.Error(src, message, title, duration, style)`
- `TG.notification.Warning(src, message, title, duration, style)`
- `TG.notification.Info(src, message, title, duration, style)`
- `TG.notification.Police(src, message, title, duration, style)`
- `TG.notification.Admin(src, message, title, duration, style)`
- `TG.notification.Announcement(src, message, keywords, title, logo)`
  - Keyword highlighting helper (exclusive to Style 1).

##### Example
```lua
TG.notification.Success(source, "You received $1,000", "Bank Transfer", 3000)
```
