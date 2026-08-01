# Client Side
You can easily call this function inside your resource script by doing `TG.alertdialog`.

### TG.alertdialog (or TG.alertdialog.Show)
----
Displays an alert/confirm modal dialog to the player.

##### Parameters:
- `data`: (table) Config properties:
  - `header`: (string) Title header text shown at the top of the dialog.
  - `content`: (string) Body description text message.
  - `cancel`: (boolean) Whether to show a cancel dismiss button.
  - `confirmText`: (string) Optional. Custom text label for the confirmation button.
  - `cancelText`: (string) Optional. Custom text label for the cancel button.
- `cb`: (function) Optional. Asynchronous callback function `function(confirmed: boolean)` triggered when the player interacts.

##### Return:
- `boolean | nil`: If no callback function is passed, blocks the execution thread and returns `true` (if confirmed) or `false`/`nil` (if cancelled).

##### Example (Synchronous/Await Mode)
```lua
local confirmed = TG.alertdialog({
    header = 'Purchase Vehicle',
    content = 'Are you sure you want to purchase this vehicle for $25,000?',
    cancel = true,
    confirmText = 'Yes, Buy It',
    cancelText = 'Cancel'
})

if confirmed then
    print("Player confirmed purchase!")
end
```

##### Example (Callback Mode)
```lua
TG.alertdialog({
    header = 'Warning',
    content = 'You are about to clear all items from your bag.',
    cancel = true
}, function(confirmed)
    if confirmed then
        print("Inventory cleared!")
    end
end)
```
