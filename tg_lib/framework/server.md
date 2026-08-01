# Server Side
You can easily call all of these functions inside your resource script by doing `TG.Core`.

> [!TIP]
> You can query `TG.Core.framework` to determine which framework is running (returns `'esx'` or `'qbx'`).

---

## General Core APIs

### TG.Core.GetPlayer
----
Returns the native framework player object (ESX `xPlayer` or QBox `Player` object).

##### Parameters:
- `source`: (number) The player's server ID.

##### Return:
- `table | boolean`: The player object, or `false` if not found.

{% hint style="warning" %}
The structure and methods of the returned player object vary according to the running framework (e.g. `xPlayer.getGroup()` under ESX vs `Player.Functions.AddMoney()` under QBox).
{% endhint %}

##### Example
```lua
local player = TG.Core.GetPlayer(source)
```


### TG.Core.HasPerms
----
Checks whether the player source possesses the given administrative group or permission.

##### Parameters:
- `source`: (number) The player's server ID.
- `perm`: (string) Permission node or group name (e.g. `'group.admin'`, `'admin'`).

##### Return:
- `boolean`: `true` if permitted, `false` otherwise.

##### Example
```lua
local isAllowed = TG.Core.HasPerms(source, 'admin')
```


### TG.Core.GetPlayers
----
Returns all players currently online formatted for administrative panel listings.

##### Return:
- `table`: Array of player elements `{ label, name, id, value, sex, description, icon }`. The `.sex` property is standardized to `'m'` or `'f'`.

##### Example
```lua
local players = TG.Core.GetPlayers()
```


### TG.Core.GetPlayerName
----
Returns the standardized character/player name.

##### Parameters:
- `source`: (number) The player's server ID.

##### Return:
- `string | boolean`: Player name string, or `false`.

##### Example
```lua
local charName = TG.Core.GetPlayerName(source)
```


### TG.Core.GetPlayerJob
----
Retrieves the player's current job details.

##### Parameters:
- `source`: (number) The player's server ID.

##### Return:
- `table | boolean`: The job object table, or `false`.
  - **ESX fields**: `.name`, `.label`, `.grade`, `.grade_name`, `.grade_label`, `.grade_salary`.
  - **QBox fields**: `.name`, `.label`, `.grade` (table with `.name`, `.level`), `.payment`, `.onduty`.

##### Example
```lua
local job = TG.Core.GetPlayerJob(source)
```


### TG.Core.GetPlayerAccount
----
Retrieves the balance of a player's cash or bank account.

##### Parameters:
- `source`: (number) The player's server ID.
- `account`: (string) Account name (e.g. `'money'`, `'bank'`). On QBox, `'money'` maps to `'cash'`.

##### Return:
- `number | boolean`: Account balance, or `false`.

##### Example
```lua
local bankBalance = TG.Core.GetPlayerAccount(source, 'bank')
```


### TG.Core.AddMoney
----
Deposits cash or bank money to a player.

##### Parameters:
- `source`: (number) The player's server ID.
- `account`: (string) Account name. On QBox, `'money'` maps to `'cash'`.
- `amount`: (number) Money to add.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

##### Example
```lua
TG.Core.AddMoney(source, 'bank', 1000)
```


### TG.Core.RemoveMoney
----
Deducts cash or bank money from a player.

##### Parameters:
- `source`: (number) The player's server ID.
- `account`: (string) Account name. On QBox, `'money'` maps to `'cash'`.
- `amount`: (number) Money to remove.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

##### Example
```lua
TG.Core.RemoveMoney(source, 'bank', 500)
```


### TG.Core.GetJobs
----
Returns a sorted list of all registered jobs on the server.

##### Return:
- `table`: Map of jobs, including their grades array.

##### Example
```lua
local jobs = TG.Core.GetJobs()
```


### TG.Core.SetJob
----
Sets the job and grade of a player.

##### Parameters:
- `source`: (number) Player server ID.
- `job`: (string) Job name.
- `grade`: (number) Job grade level.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

##### Example
```lua
TG.Core.SetJob(source, 'police', 2)
```

---

## Vehicle Management APIs

### TG.Core.PlateExist
----
Verifies whether a vehicle plate already exists in the database.

##### Parameters:
- `plate`: (string) License plate string.

##### Return:
- `boolean`: `true` if it exists, `false` otherwise.

```lua
local exists = TG.Core.PlateExist('TGLIB')
```


### TG.Core.AddVehicleToDatabase
----
Saves a new vehicle to the database under the player's ownership and spawns it.
- **ESX Database**: Inserts into `owned_vehicles`.
- **QBox Database**: Inserts into `players_vehicles` via `qbx_vehicles`.

##### Parameters:
- `src`: (number) Owner player server ID.
- `model`: (string) Vehicle model.
- `plate`: (string) Vehicle plate.

##### Return:
- `boolean`: `true` if successfully added and spawned, `false` otherwise.

```lua
TG.Core.AddVehicleToDatabase(source, 'adder', 'TGLIB')
```


### TG.Core.updateVehicle
----
Updates vehicle custom property JSON modifications in the database (applicable under **ESX**; immediately fires callback with `true` under **QBox**).

##### Parameters:
- `plate`: (string) Vehicle plate.
- `prop`: (table) Vehicle mods properties table.
- `cb`: (function) Optional callback `function(success: boolean)`.

```lua
TG.Core.updateVehicle('TGLIB', propertiesTable)
```


### TG.Core.UpdatePlate
----
Updates the license plate string database reference (applicable under **ESX** only; no-op under **QBox**).

##### Parameters:
- `oplate`: (string) Old plate string.
- `nplate`: (string) New plate string.
- `src`: (number) Invoker player server ID for notification display.

```lua
TG.Core.UpdatePlate('OLD123', 'NEW123', source)
```


### TG.Core.SetVehicleOwner
----
Updates vehicle database ownership.
- If `src` > `0`: Transfers owner citizen ID/license index.
- If `src` == `0`: Deletes the owned vehicle entry.

##### Parameters:
- `src`: (number) Target player server ID (or `0` to delete).
- `plate`: (string) Vehicle plate.

```lua
TG.Core.SetVehicleOwner(targetPlayerId, 'TGLIB')
```


### TG.Core.GetVehicles
----
Fetches owned vehicles matching player ID and plate constraints.

##### Parameters:
- `src`: (number) Target player server ID.
- `plate`: (string) Optional. Plate filter (if matched, returns the raw vehicle row instead).

##### Return:
- `table`: Array list of owned vehicles `{ label, value, plate, state, garage }`, or the raw matching vehicle row.

```lua
local vehicles = TG.Core.GetVehicles(source)
```


### TG.Core.SpawnVehicle
----
Spawns one of the player's owned vehicles by plate.

##### Parameters:
- `src`: (number) Player server ID.
- `data`: (table) Data containing `.plate`.
- `cb`: (function) Optional callback receiving the network entity handle.

```lua
TG.Core.SpawnVehicle(source, { plate = 'TGLIB' }, function(vehicle)
    print("Spawned vehicle:", vehicle)
end)
```


### TG.Core.DeleteVehicle
----
Deletes a vehicle record from the database by its plate.

> [!IMPORTANT]
> Exclusive to **QBox** (using `qbx_vehicles`).

##### Parameters:
- `target`: (number) Player ID.
- `plate`: (string) Vehicle plate.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

```lua
TG.Core.DeleteVehicle(source, 'TGLIB')
```

---

## Gang APIs (QBox/QBCore Only)

> [!IMPORTANT]
> The gang utility APIs are exclusive to **QBox** and **QBCore** structures. Under ESX, these functions do not exist.

### TG.Core.GetPlayerGang
----
Retrieves a player's gang status.

##### Parameters:
- `source`: (number) Player server ID.

##### Return:
- `table | boolean`: Gang status object containing `.name` and `.grade`, or `false`.

```lua
local gang = TG.Core.GetPlayerGang(source)
```


### TG.Core.GetGangs
----
Returns a list of all registered gangs on the server.

##### Return:
- `table`: Gangs data table list.

```lua
local gangs = TG.Core.GetGangs()
```


### TG.Core.SetGang
----
Sets the gang and grade of a player.

##### Parameters:
- `source`: (number) Player server ID.
- `gang`: (string) Gang name.
- `grade`: (number) Gang grade level.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

```lua
TG.Core.SetGang(source, 'ballas', 1)
```

---

## Dynamic Job Management APIs (QBox/QBCore Only)

> [!IMPORTANT]
> These administrative functions are exclusive to **QBox** utilizing dynamic core edits and group integrations.

### TG.Core.getJobsAndEmploy
----
Retrieves the list of jobs along with the current online group members count.

##### Return:
- `table`: Jobs list containing `.totalPlayer` count fields.

```lua
local data = TG.Core.getJobsAndEmploy()
```


### TG.Core.createJob
----
Creates a new job dynamically in the framework registry.

##### Parameters:
- `jobName`: (string) Unique code name.
- `jobLabel`: (string) Display name.
- `grades`: (array) List of grade tables `{ grade = 0, label = 'Recruit', salary = 100 }`.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.
- `string`: Error message if unsuccessful.

```lua
local ok, err = TG.Core.createJob('mechanic', 'Auto Repairs', { { grade = 0, label = 'Apprentice', salary = 50 } })
```


### TG.Core.GetJobData
----
Fetches a detailed profile of a job, listing members online and active map configurations (blips, garages, stashes).

##### Parameters:
- `job`: (string) Job name.

##### Return:
- `table`: Job profile details `{ name, label, grades, players, dutyzones, blips, stashes, vehicles, garages }`.

```lua
local details = TG.Core.GetJobData('police')
```


### TG.Core.AddJobGrade
----
Adds or updates a grade level inside a job configuration.

##### Parameters:
- `job`: (string) Job name.
- `grade`: (number) Grade level number.
- `gradeData`: (table) Grade details `{ label, salary }`.

```lua
TG.Core.AddJobGrade('police', 3, { label = 'Lieutenant', salary = 500 })
```


### TG.Core.DeleteJobGrade
----
Deletes a grade level from a job configuration.

##### Parameters:
- `job`: (string) Job name.
- `grade`: (number) Grade level number.

```lua
TG.Core.DeleteJobGrade('police', 3)
```


### TG.Core.SetJobDuty
----
Sets the player's on-duty status.

##### Parameters:
- `source`: (number) Player server ID.
- `onDuty`: (boolean) On-duty status toggle.

```lua
TG.Core.SetJobDuty(source, true)
```