# Client Side
You can easily call all of these functions inside your resource script by doing `TG.TextUI`.

### TG.TextUI (or TG.TextUI.Show)
----
Renders a persistent TextUI HUD indicator block on the screen (often used for interactable hotkey warnings).

##### Parameters:
- `text`: (string) Display warning text content (supports string styling).
- `options`: (table) Config properties:
  - `icon`: (string) Optional. Lucide icon name.
  - `iconColor`: (string) Optional. Custom icon hex color code.
  - `iconAnimation`: (string) Optional. Icon animation type: `'spin'` | `'pulse'` | `'beat'` | `'fade'` | `'bounce'`.
  - `position`: (string) Optional. Position: `'right-center'` | `'left-center'` | `'top-center'` | `'bottom-center'`.
  - `style`: (number | table) Optional. Visual style number layout `1` | `2` | `3` | `4`, or a table of custom CSS styles.
  - `customStyle`: (table) Optional. Key-value table of custom CSS parameters.

##### Example
```lua
TG.TextUI('[E] - Access Warehouse', {
    icon = 'icon-key-round',
    iconAnimation = 'bounce',
    position = 'right-center',
    style = 1
})
```


### TG.TextUI.Hide
----
Hides the active TextUI HUD indicator.

##### Example
```lua
TG.TextUI.Hide()
```


### TG.TextUI.IsOpen
----
Checks whether the TextUI HUD indicator is currently visible on the player's screen.

##### Return:
- `boolean`: `true` if currently open, `false` otherwise.

##### Example
```lua
local open = TG.TextUI.IsOpen()
```


### TG.TextUI.GetText
----
Retrieves the text string currently shown in the TextUI indicator.

##### Return:
- `string | nil`: Text content, or `nil` if closed.

##### Example
```lua
local text = TG.TextUI.GetText()
```
