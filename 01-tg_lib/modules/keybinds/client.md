# Client Side
You can easily call all of these functions inside your resource script by doing `TG.keybinds`.

### TG.keybinds.add
----
Registers a game keybind using FiveM's native RegisterKeyMapping API.

##### Parameters:
- `data`: (table) Config properties:
  - `name`: (string) Unique code name identifier (registers commands `+name` and `-name`).
  - `description`: (string) Text shown in setting key-bindings menu.
  - `defaultMapper`: (string) Default mapper device (defaults to `'keyboard'`).
  - `defaultKey`: (string) Default binding key (e.g. `'E'`, `'F5'`).
  - `secondaryKey`: (string) Optional alternate shortcut key.
  - `onPressed`: (function) Callback executed when keybind is pressed. Receives `self` (bind object).
  - `onReleased`: (function) Callback executed when keybind is released. Receives `self` (bind object).

##### Return:
- `table`: The registered `bind` object.

##### Example
```lua
local keybind = TG.keybinds.add({
    name = 'open_inventory',
    description = 'Open Inventory Menu',
    defaultKey = 'I',
    onPressed = function(self)
        print("Inventory key pressed!")
    end,
    onReleased = function(self)
        print("Inventory key released!")
    end
})
```


### bind:disable
----
Temporarily disables or enables execution of the key mapping callbacks.

##### Parameters:
- `toggle`: (boolean) Set to `true` to block execution, `false` to reactivate.

##### Example
```lua
keybind:disable(true) -- blocks input detection
```


### bind:isControlPressed
----
Checks whether the key is currently held down.

##### Return:
- `boolean`: `true` if pressed, `false` otherwise.

##### Example
```lua
local isHeld = keybind:isControlPressed()
```


### bind.currentKey
----
Retrieves the name string of the currently mapped key button.

##### Return:
- `string`: Key button string.

##### Example
```lua
print("Mapped key button:", keybind.currentKey)
```
