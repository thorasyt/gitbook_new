# Client Side
You can easily call all of these functions inside your resource script by doing `TG.inputDialog`.

### TG.inputDialog (or TG.inputDialog.Show)
----
Displays an input form dialog modal on the player's screen.

##### Parameters:
- `heading`: (string) Title header text shown at the top of the form modal.
- `rows`: (array) A list of input fields/row tables:
  - `type`: (string) Field type: `'input'` | `'number'` | `'checkbox'` | `'select'` | `'multiselect'` | `'slider'` | `'color'` | `'date'` | `'time'` | `'textarea'`.
  - `label`: (string) Label text shown above the field.
  - `description`: (string) Optional. Subtext description.
  - `icon`: (string) Optional. Icon name (Lucide icons).
  - `placeholder`: (string) Optional. Placeholder text.
  - `default`: (any) Optional. Default value.
  - `required`: (boolean) Optional. Prevents submission if the field is empty.
  - `disabled`: (boolean) Optional. Disables editing the field.
  - `min`/`max`: (number) Optional. Min/max range constraints for numbers/sliders or length constraints for strings.
  - `step`: (number) Optional. Value step increments for numbers/sliders.
  - `options`: (array) Optional. Array of selection tables `{ label = 'Male', value = 'm' }` (for `'select'` and `'multiselect'` fields).
- `options`: (table) Optional. Config settings for the modal dialog window:
  - `allowCancel`: (boolean) Whether the player can cancel/dismiss the form (default `true`).
- `cb`: (function) Optional. Asynchronous callback function `function(result: table | nil)` triggered on form submission.

##### Return:
- `table | nil`: If no callback function is passed, blocks the execution thread and returns the result `table` on submission, or `nil` if cancelled.
  - The returned table maps keys (the index of the row, e.g., `result[1]` for the first row, or a custom string index if specified in the row) to the values input/selected by the player.

##### Example (Synchronous/Await Mode)
```lua
local result = TG.inputDialog('Create Character', {
    { type = 'input', label = 'First Name', placeholder = 'John', required = true },
    { type = 'input', label = 'Last Name', placeholder = 'Doe', required = true },
    { type = 'number', label = 'Age', default = 21, min = 1, max = 150 },
    { type = 'select', label = 'Gender', default = 'm', options = {
        { label = 'Male', value = 'm' },
        { label = 'Female', value = 'f' }
    }}
})

if result then
    local firstName = result[1]
    local lastName = result[2]
    local age = result[3]
    local gender = result[4]
    print(string.format("Character Created: %s %s, Age: %d, Gender: %s", firstName, lastName, age, gender))
end
```


### TG.inputDialog.Hide
----
Forcefully hides and dismisses the currently open input dialog form.

##### Example
```lua
TG.inputDialog.Hide()
```
