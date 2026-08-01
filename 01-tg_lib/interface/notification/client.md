# Client Side
You can easily call all of these functions inside your resource script by doing `TG.notification`.

### TG.notification (or TG.notification.Show)
----
Renders a toast notification on the screen.

##### Parameters:
- `data`: (table) Config properties:
  - `title`: (string) Optional. Header title text.
  - `message`: (string) Main message body text description.
  - `type`: (string) Optional. Type: `'success'` | `'error'` | `'warning'` | `'info'` | `'police'` | `'admin'` | `'custom'` (default `'info'`).
  - `duration`: (number) Optional. Display duration in milliseconds (default `5000`).
  - `style`: (number) Optional. Layout design theme style index `1` | `2` | `3` | `4` | `5` | `6` (default `1`):
    - **Style 1**: Modern dark with rings progress indicator, keyword highlights, and logos.
    - **Style 2**: Charcoal layout with bottom progress bars and timestamps.
    - **Style 3**: Vibrant colored pills with solid backgrounds.
    - **Style 4**: Modern glass blur effect with gradient borders.
    - **Style 5**: Minimal layout with accents line highlighting.
    - **Style 6**: Toast bar with custom dismiss buttons.
  - `icon`: (string) Optional. Lucide icon name.
  - `iconColor`: (string) Optional. Hex color string.
  - `keywords`: (array) Optional. Array of keyword structures `{ text = 'highlight', color = '#ff0000' }` (Style 1 only).
  - `logo`: (string) Optional. Custom logo image URL (Style 1 only).
  - `showTimestamp`: (boolean) Optional. Shows clock timestamp (Style 2 only).
  - `accentColor`: (string) Optional. Hex color code.
  - `progress`: (boolean) Optional. Shows the progress timeout bar (default `true`).
  - `silent`: (boolean) Optional. Mutes notification sound alerts.

##### Return:
- `string`: Unique notification ID.

##### Example
```lua
local notifId = TG.notification({
    title = 'Bag Full',
    message = 'You cannot carry any more items in your inventory.',
    type = 'warning',
    style = 4,
    icon = 'icon-alert-triangle'
})
```


### TG.notification.Remove
----
Removes and dismisses a specific notification immediately by its ID.

##### Parameters:
- `id`: (string) Notification unique ID.

##### Example
```lua
TG.notification.Remove(notifId)
```


### TG.notification.SetPosition
----
Sets the layout alignment of active notifications on the screen.

##### Parameters:
- `position`: (string) Position align: `'left-top'` | `'left-center'` | `'left-bottom'` | `'right-top'` | `'right-center'` | `'right-bottom'` | `'top-center'` | `'bottom-center'`.

##### Example
```lua
TG.notification.SetPosition('right-top')
```


### Shorthand Helpers
Shorthand client APIs mapping visual types.
- `TG.notification.Success(message, title, duration)`
- `TG.notification.Error(message, title, duration)`
- `TG.notification.Warning(message, title, duration)`
- `TG.notification.Info(message, title, duration)`
- `TG.notification.Police(message, title, duration)`
- `TG.notification.Admin(message, title, duration)`
- `TG.notification.Announcement(message, keywords, title, logo)`
  - Keyword highlight parser (exclusively uses Style 1).

##### Example
```lua
TG.notification.Success("Your item was sold successfully!", "Marketplace")
```
