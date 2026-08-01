# Client Side
You can easily call all of these functions inside your resource script by doing `TG.discord`.

### TG.discord.GetAvatar
----
Retrieves the invoking client's own Discord avatar URL.

##### Return:
- `string`: The avatar image HTTP URL.

##### Example
```lua
local myAvatarUrl = TG.discord.GetAvatar()
print("My avatar is:", myAvatarUrl)
```
