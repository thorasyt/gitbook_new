# Client Side
You can call `TG.permission` directly as a function to check user permissions.

### TG.permission
----
Queries the server-side callback to verify player permissions.

##### Parameters:
- `permission`: (string) The permission node to query.

##### Return:
- `boolean`: `true` if the player has permission, `false` otherwise.

##### Example
```lua
local hasPermission = TG.permission('admin.kick')
if hasPermission then
    -- Show admin panel controls
end
```
