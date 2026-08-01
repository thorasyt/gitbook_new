# Client Side
You can easily call all of these functions inside your resource script by doing `TG.announcement`.

### TG.announcement (or TG.announcement.Show)
----
Renders a persistent HUD announcement banner on the local player's screen.

##### Parameters:
- `data`: (table) Config properties:
  - `type`: (string) Announcement type: `'staff'` | `'server'` | `'dm'`.
  - `title`: (string) Heading banner title text.
  - `message`: (string) Body description text message.
  - `sender`: (string) Optional. Sender display name (only used for `'dm'` type).
  - `duration`: (number) Optional. Auto-dismiss timeout in milliseconds (default `8000`).
  - `img`: (string) Optional. Custom logo/image URL path.

##### Return:
- `string`: A unique announcement ID (useful for manually dismissing the banner early).

##### Example
```lua
local announceId = TG.announcement({
    type = 'server',
    title = 'Event Started',
    message = 'The race at LS International Airport has started!',
    duration = 5000
})
```


### TG.announcement.Hide
----
Forcefully hides and dismisses an active announcement banner before its duration expires.

##### Parameters:
- `id`: (string) The unique announcement ID.

##### Example
```lua
TG.announcement.Hide(announceId)
```


### TG.announcement.Staff
----
Shorthand helper to show a `'staff'` style announcement.

##### Parameters:
- `title`: (string) Header title text.
- `message`: (string) Description message.
- `duration`: (number) Display duration in milliseconds.

##### Example
```lua
TG.announcement.Staff('Admin Notice', 'Please refrain from using vehicles in this zone.', 5000)
```


### TG.announcement.Server
----
Shorthand helper to show a `'server'` style announcement.

##### Parameters:
- `title`: (string) Header title text.
- `message`: (string) Description message.
- `duration`: (number) Display duration in milliseconds.

##### Example
```lua
TG.announcement.Server('Server Message', 'Weekly server maintenance will start in 10 minutes.', 10000)
```


### TG.announcement.DM
----
Shorthand helper to show a `'dm'` (Direct Message) style announcement.

##### Parameters:
- `title`: (string) Header title text.
- `message`: (string) Description message.
- `sender`: (string) The sender's name display.
- `duration`: (number) Display duration in milliseconds.

##### Example
```lua
TG.announcement.DM('New Message', 'Hey, meet me at the warehouse.', 'John Doe', 6000)
```
