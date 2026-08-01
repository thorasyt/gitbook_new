# Server Side
The library overrides the global `GetPlayerName` function to use this module.

### GetPlayerName (Global Replacement)
----
Fetches the display name of a target player ID. On the server side, this resolves the player's Discord profile name (nickname, global name, or username) or falls back to their FiveM connection name.

##### Parameters:
- `id`: (number) Player server ID.

##### Return:
- `string`: Player name.

##### Example
```lua
local name = GetPlayerName(playerId)
```
