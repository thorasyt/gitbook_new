# Client Side
You can easily call all of these functions inside your resource script by doing `TG.menu`.

### TG.menu.Register
----
Registers a list menu definition with options and selection callbacks.

##### Parameters:
- `data`: (table) Config properties:
  - `id`: (string) Unique menu identifier ID.
  - `title`: (string) Header title text shown at the top of the menu.
  - `subtitle`: (string) Subheading description text.
  - `canClose`: (boolean) Optional. Whether the player can close the menu via Escape or key binds (default `true`).
  - `startIndex`: (number) Optional. The default selected option index when opened (default `1`).
  - `disableInput`: (boolean) Optional. Set to `true` to block player gun firing/controls while the menu is open (default `true`).
  - `onClose`: (function) Optional. Callback function executed when the menu is closed.
  - `onSelected`: (function) Optional. Callback function executed whenever option highlights change: `function(index, currentType, ...args)`.
  - `onCheck`: (function) Optional. Callback function executed when toggle checkboxes are modified: `function(index, checked, ...args)`.
  - `onSliderChange`: (function) Optional. Callback function executed when sliders are changed: `function(index, value, ...args)`.
  - `options`: (array) A list of option configuration tables:
    - `label`: (string) Item display text.
    - `description`: (string) Optional. Description text.
    - `icon`: (string) Optional. Lucide icon name.
    - `disabled`: (boolean) Optional. Disables clicking the item.
    - `close`: (boolean) Optional. Whether the menu closes automatically when clicked (default `true`).
    - `toggle`: (boolean) Optional. Renders a toggle checkbox on the item.
    - `checked`: (boolean) Optional. Default toggle checkbox state.
    - `slider`: (boolean) Optional. Renders a numeric range slider on the item.
    - `min`/`max`/`step`: (number) Optional. Slider limits and stepping.
    - `values`: (array) Optional. Renders a side-scrolling list of select values.
    - `progress`: (number) Optional. Renders a visual progress bar index.
    - `args`: (table) Optional. Arguments table passed to selection callbacks.
- `cb`: (function) Selection callback function triggered when options are confirmed/clicked: `function(index, currentType, args, controlType)`.

##### Example
```lua
TG.menu.Register({
    id = 'vehicle_customs',
    title = 'Customs Shop',
    subtitle = 'Upgrade Your Vehicle',
    options = {
        { label = 'Repair Vehicle', args = { 'repair' } },
        { label = 'Upgrade Engine', description = 'Increase top speed', args = { 'engine_mod' } },
        { label = 'Toggle Neon Lights', toggle = true, checked = false, args = { 'neon_lights' } },
        { label = 'Suspension Level', slider = true, min = 1, max = 5, step = 1, args = { 'suspension' } }
    },
    onCheck = function(index, checked, arg)
        print("Toggled index:", index, "Checked:", checked, "Arg:", arg)
    end,
    onSliderChange = function(index, value, arg)
        print("Slider changed index:", index, "Value:", value, "Arg:", arg)
    end
}, function(index, current, args, controlType)
    local action = args[1]
    print("Player selected option index:", index, "Action:", action)
end)
```


### TG.menu (or TG.menu.Show)
----
Displays a registered list menu on the screen by its ID.

##### Parameters:
- `id`: (string) The registered menu ID.

##### Example
```lua
TG.menu('vehicle_customs')
```


### TG.menu.Hide
----
Forcefully hides and closes the currently open list menu.

##### Parameters:
- `onExit`: (boolean) Optional. If set to `true`, triggers the menu's custom `onClose` callback.

##### Example
```lua
TG.menu.Hide(true)
```


### TG.menu.SetPosition
----
Changes the screen alignment position of the menu window.

##### Parameters:
- `position`: (string) Screen layout alignment: `'left-top'` | `'left-center'` | `'left-bottom'` | `'right-top'` | `'right-center'` | `'right-bottom'` | `'center'`.

##### Example
```lua
TG.menu.SetPosition('left-center')
```


### TG.menu.getOpenMenu
----
Retrieves the open menu's data structure and ID.

##### Return:
- `table`: Menu configuration table data.
- `string`: Menu unique ID.

##### Example
```lua
local menuData, menuId = TG.menu.getOpenMenu()
```


### TG.menu.SetOptions
----
Dynamically updates the options list of a registered menu.

##### Parameters:
- `id`: (string) Target menu ID.
- `options`: (array) New array list of options.

##### Example
```lua
TG.menu.SetOptions('vehicle_customs', {
    { label = 'New Option 1', args = { 1 } }
})
```


### TG.menu.AddEditOption
----
Appends a new option, or updates/overwrites an option at a specific index index in a registered menu.

##### Parameters:
- `id`: (string) Target menu ID.
- `option`: (table) The option configuration properties.
- `index`: (number) Optional. Target array position. If omitted or `0`, appends to the end of the options list.

##### Example
```lua
TG.menu.AddEditOption('vehicle_customs', { label = 'Added Option', args = { 'added' } }, 0)
```
