# Client Side
You can easily call all of these functions inside your resource script by doing `TG.contextMenu`.

### TG.contextMenu.Register
----
Registers a context menu definition configurations.

##### Parameters:
- `data`: (table) Config properties:
  - `id`: (string) Unique menu identifier ID.
  - `title`: (string) Title header text shown at the top of the menu.
  - `canClose`: (boolean) Optional. Whether the player can close the menu by pressing Escape or clicking outside (default `true`).
  - `position`: (string) Optional. Position: `'left'` | `'right'` (default `'right'`).
  - `options`: (array) A list of option item tables:
    - `title`: (string) Item title text.
    - `description`: (string) Optional. Subtext description.
    - `icon`: (string) Optional. Icon name (Lucide icons library).
    - `iconColor`: (string) Optional. Color code (hex) for the icon.
    - `iconAnimation`: (string) Optional. Icon animation type: `'spin'` | `'pulse'` | `'beat'` | `'fade'` | `'bounce'`.
    - `image`: (string) Optional. Custom image URL displayed on the item.
    - `arrow`: (boolean) Optional. Displays a right arrow indicator.
    - `progress`: (number) Optional. Progress bar percentage (value `0`-`100`).
    - `colorScheme`: (string) Optional. Color hex for the progress bar.
    - `readOnly`: (boolean) Optional. Makes the item non-clickable.
    - `disabled`: (boolean) Optional. Disables clicking on the item.
    - `close`: (boolean) Optional. Whether the menu closes automatically when this option is clicked (default `true`).
    - `menu`: (string) Optional. Submenu ID that opens when this option is clicked.
    - `onSelect`: (function) Optional. Callback function executed when this option is selected.
    - `event`: (string) Optional. Client event triggered on selection.
    - `serverEvent`: (string) Optional. Server event triggered on selection.
    - `args`: (any) Optional. Arguments passed to the callback or events.

##### Example
```lua
TG.contextMenu.Register({
    id = 'character_actions',
    title = 'Character Interaction',
    options = {
        {
            title = 'Show ID Card',
            description = 'Present your identification card to nearby players',
            icon = 'icon-card',
            onSelect = function()
                print("Showing ID Card...")
            end
        },
        {
            title = 'Submenu Actions',
            description = 'Open another submenu list',
            icon = 'icon-chevron-right',
            menu = 'submenu_actions'
        },
        {
            title = 'Restricted Option',
            description = 'You cannot click this option',
            disabled = true,
            icon = 'icon-lock'
        }
    }
})
```


### TG.contextMenu (or TG.contextMenu.Show)
----
Displays a registered context menu by its ID on the screen.

##### Parameters:
- `id`: (string) The registered menu ID.

##### Example
```lua
TG.contextMenu('character_actions')
```


### TG.contextMenu.hideContextMenu
----
Forcefully hides and closes the currently open context menu.

##### Parameters:
- `onExit`: (boolean) Optional. If set to `true`, triggers the menu's custom `onExit` callback.

##### Example
```lua
TG.contextMenu.hideContextMenu(true)
```


### TG.contextMenu.getOpenContetMenu
----
Retrieves the ID of the context menu currently displayed on the player's screen.

##### Return:
- `string | nil`: The menu ID, or `nil` if no context menu is currently open.

##### Example
```lua
local openMenuId = TG.contextMenu.getOpenContetMenu()
```
