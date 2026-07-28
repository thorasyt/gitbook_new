# Server Side
you can easily call all of this function inside of your resource script by doing `TG.Core`

### TG.Core.GetPlayer
----
this function will return playerdata object of specified player by id or source.
##### parameters:
- source: (number) the player's server side id

##### return:
- table: playerdata object

{% hint style="warning" %}
result may vary according to framework.
{% endhint %}

##### Example
```lua
local player = TG.Core.GetPlayer(source)
if player then
   print(json.encode(player))
end
```
