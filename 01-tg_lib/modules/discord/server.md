# Server Side
You can easily call all of these functions inside your resource script by doing `TG.discord`.

### TG.discord.GetUser
----
Returns raw guild member information table retrieved from the Discord Web API endpoint.

##### Parameters:
- `id`: (number) Player's server ID.

##### Return:
- `table | boolean`: Discord guild member object table, or `false` if query failed.

##### Example
```lua
local profile = TG.discord.GetUser(source)
if profile then
    print("Discord Nickname:", profile.nick)
    print("Username:", profile.user.username)
end
```


### TG.discord.GetAvatar
----
Returns the Discord avatar URL for the player (resolves GIFs for animated avatars, falls back to default Discord avatar if not found).

##### Parameters:
- `id`: (number) Player's server ID.

##### Return:
- `string`: URL string.

##### Example
```lua
local avatarUrl = TG.discord.GetAvatar(source)
```


### TG.discord.GetName
----
Resolves user's Discord display name (server nickname, global name, or username) or falls back to their FiveM game name.

##### Parameters:
- `id`: (number) Player's server ID.

##### Return:
- `string`: The resolved display name.

##### Example
```lua
local name = TG.discord.GetName(source)
```
