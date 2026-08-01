# Client Side
The library overrides the global `GetPlayerName` function to use this module.

### GetPlayerName (Global Replacement)
----
Fetches the display name of a target player ID. On the client side, this queries a server-side callback to resolve the player's Discord profile name.

##### Parameters:
- `id`: (number) Player server ID.

##### Return:
- `string`: Player name.

##### Example
```lua
local name = GetPlayerName(playerId)
```
