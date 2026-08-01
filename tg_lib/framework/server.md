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


### TG.Core.HasPerms
-------
This function returns a boolean indicating whether the player has the specified permission.

##### parameters:
- source: (number) the player's server side id
- perms: (string) the permission to check

##### return:
- boolean: true if the player has the permission, false otherwise

{% hint style="info" %}
This function checks the player's framework permissions instead of ACE permissions.
{% endhint %}

##### Example
```lua
local hasPerms = TG.Core.HasPerms(source, "admin.ban")
if hasPerms then
   print("Player has permission")
end
```


### TG.Core.GetPlayers
----
this function will return entire players inside server.

##### return 
- table: array of playerdata object

{% hint style="info" %}
This function is build for our on scripts
{% endhint %}


##### Example
```lua
local players = TG.Core.GetPlayers()
for i = 1, #players do
    local player = players[i]
    print(player.name || player.label) -- label has player id 
    print(player.id || player.value)
    print(player.sex)
end
```

### TG.Core.GetPlayerJob
----
This function will return player job using player Id

##### parameters
- source: (number) the player's server side id

##### return
- table: player job object

{% hint style="warning" %}
result may vary according to framework.
{% endhint %}


##### Example
```lua
local playerJob = TG.Core.GetPlayerJob(source)
if playerJob then
   print(json.encode(playerJob))
end
```

### TG.Core.GetPlayerName
----
this function will return player name using player Id

##### parameters
- source: (number) the player's server side id

##### return
- string: player name

##### Example
```lua
local playerName = TG.Core.GetPlayerName(source)
if playerName then
   print(playerName)
end
```

### TG.Core.GetPlayerAccount
----
This function will return player specified account's money using  player Id

##### parameters
- source: (number) the player's server side id
- accountName: (string) the account name

##### return
- number: player account's money

##### Example
```lua
local playerAccount = TG.Core.GetPlayerAccount(source, "money")
if playerAccount then
   print(playerAccount)
end
```

### TG.Core.AddMoney
----
This function will add money to player's specified account using player Id

##### parameters
- source: (number) the player's server side id
- accountName: (string) the account name
- amount: (number) the amount of money to add

##### return
- boolean: true if the money was added successfully, false otherwise

##### Example
```lua
local playerAccount = TG.Core.AddMoney(source, "money", 100)
if playerAccount then
   print("Money added successfully")
end
```


### TG.Core.RemoveMoney
----
This function will remove money from player's specified account using player Id

##### parameters
- source: (number) the player's server side id
- accountName: (string) the account name
- amount: (number) the amount of money to remove

##### return
- boolean: true if the money was removed successfully, false otherwise

##### Example
```lua
local playerAccount = TG.Core.RemoveMoney(source, "money", 100)
if playerAccount then
   print("Money removed successfully")
end
```


### TG.Core.GetJobs
----
This function will return all jobs inside the server

##### return
- table: array of job objects

##### Example
```lua
local jobs = TG.Core.GetJobs()
for i = 1, #jobs do
    local job = jobs[i]
    print(job.value) 
    print(job.label) 
    print(#job.grades)
end
```


### TG.Core.SetJob
----
This function will set job of player using player Id

##### parameters
- source: (number) the player's server side id
- jobName: (string) the job name
- grade: (number) the grade of the job

##### return
- boolean: true if the job was set successfully, false otherwise

##### Example
```lua
local playerJob = TG.Core.SetJob(source, "police", 0)
if playerJob then
   print("Job set successfully")
end
```


### TG.Core.PlateExist
-----
This function will return a boolen value true if plate exist in database otherwise false

##### parameters
- plate: (string) the plate to check

##### return
- boolean: true if the plate exists, false otherwise

##### Example
```lua
local plateExist = TG.Core.PlateExist("123456")
if plateExist then
   print("Plate exists")
end
```


### TG.Core.AddVehicleToDatabase
----
This function will execute vehicle spawn and save properties to database then return boolen value.

##### parameters
- src: (number) the player's server side id
- model: (string) the vehicle model
- plate: (string) the vehicle plate

##### return
- boolean: true if the vehicle was added successfully, false otherwise

##### Example
```lua
local vehicleAdded = TG.Core.AddVehicleToDatabase(source, "Adder", "123456")
if vehicleAdded then
   print("Vehicle added successfully")
end
```


### 