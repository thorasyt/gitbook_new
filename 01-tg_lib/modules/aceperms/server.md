# Server Side
You can easily call all of these functions inside your resource script by doing `TG.permission`.

### TG.permission.addAce
----
Dynamically executes the FiveM console command `add_ace <principle> <ace> <allow/deny>`.

##### Parameters:
- `principle`: (string | number) The principal group (e.g. `'group.admin'`) or player ID (number, which gets automatically formatted to `'player.<id>'`).
- `ace`: (string) The permission node to add (e.g. `'command.kick'`).
- `allow`: (boolean) Set to `true` to allow, `false` to deny.

##### Example
```lua
TG.permission.addAce(source, 'command.kick', true)
```


### TG.permission.removeAce
----
Dynamically executes the FiveM console command `remove_ace <principle> <ace> <allow/deny>`.

##### Parameters:
- `principle`: (string | number) The principal group or player ID (number).
- `ace`: (string) The permission node.
- `allow`: (boolean) Permission state to remove.

##### Example
```lua
TG.permission.removeAce(source, 'command.kick', true)
```


### TG.permission.addPrincipal
----
Maps a child principal (such as a player ID principal) to a parent principal group. Executes `add_principal <child> <parent>`.

##### Parameters:
- `child`: (string | number) The child principal or player ID (number).
- `parent`: (string | number) The parent principal group or player ID (number).

##### Example
```lua
TG.permission.addPrincipal(source, 'group.admin')
```


### TG.permission.removePrincipal
----
Removes a principal inheritance mapping. Executes `remove_principal <child> <parent>`.

##### Parameters:
- `child`: (string | number) The child principal.
- `parent`: (string | number) The parent principal group.

##### Example
```lua
TG.permission.removePrincipal(source, 'group.admin')
```


### TG.permission.hasPermission
----
Checks whether the player source possesses the given permission node via FiveM Ace check or framework-level permission check.

##### Parameters:
- `source`: (number) The player's server ID.
- `permission`: (string) The permission node string to check.

##### Return:
- `boolean`: `true` if allowed, `false` otherwise.

##### Example
```lua
local allowed = TG.permission.hasPermission(source, 'admin.menu')
```
