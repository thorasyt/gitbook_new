# Client Side
you can easily call all of this function inside of your resource script by doing `TG.Core`
### TG.Core.getPlayerData
----
this will return player data which will helpful for you to check player information in your script. This will change according to framework of server. 

```lua
local playerdata = TG.Core.getPlayerData()
```

### TG.Core.getPlayerNameClient
----
this function will return the player name.

```lua
local playername = TG.Core.getPlayerNameClient()
```

### TG.Core.GetJob
----
this function will return the player job which contain job name, label, grade, etc...

```lua
local job = TG.Core.GetJob()
```

### TG.Core.GetJobDuty
----
this function will return the player job duty status

```lua
local duty = TG.Core.GetJobDuty()
```

### TG.Core.VehicleList
----
this function will return entire vehicles inside server

```lua
local vehicles = TG.Core.VehicleList()
```

> [!IMPORTANT]
> If you need know which framework or need to refer which one in script use ```local framework = TG.Core.framework```