# Server Side
You can call this function inside your resource script by doing `TG.announcement`.

### TG.announcement.Show
----
Triggers a full-screen or banner notification alert on the target player's screen.

##### Parameters:
- `src`: (number) The target player's server ID.
- `data`: (table) Config properties:
  - `type`: (string) Announcement type: `'staff'` | `'server'` | `'dm'`.
  - `title`: (string) The title banner text.
  - `message`: (string) Main description message text.
  - `sender`: (string) Optional. Sender name (displayed for `'dm'` announcements).
  - `duration`: (number) Optional. Display duration in milliseconds (default `8000`).
  - `img`: (string) Optional. URL path of the sender image.

##### Example
```lua
TG.announcement.Show(source, {
    type = 'server',
    title = 'Server Restart',
    message = 'The server is restarting in 5 minutes for scheduled maintenance.',
    duration = 10000
})
```
