# Client Side
You can easily call all of these functions inside your resource script by doing `TG.Core`.

> [!TIP]
> You can query `TG.Core.framework` to determine which framework is running (returns `'esx'` or `'qbx'`).

---

## Client Core APIs

### TG.Core.getPlayerData
----
Returns the current cached player data block.

##### Return:
- `table`: The framework's player data structure.

##### Example
```lua
local data = TG.Core.getPlayerData()
```


### TG.Core.getPlayerNameClient
----
Retrieves the local player's character name string.

##### Return:
- `string`: Player character display name (e.g. `'John Doe'`).

##### Example
```lua
local charName = TG.Core.getPlayerNameClient()
```


### TG.Core.GetJob
----
Retrieves the local player's current job details.

##### Return:
- `table | nil`: Job details table.
  - **ESX fields**: Contains `.name`, `.label`, `.grade`, `.grade_name`, `.grade_label`, `.grade_salary`, etc.
  - **QBox fields**: Contains `.name`, `.label`, `.grade` (table with `.name`, `.level`), `.payment`, `.onduty`, etc.

##### Example
```lua
local job = TG.Core.GetJob()
if job then
    print("My job is:", job.name)
end
```


### TG.Core.GetJobDuty
----
Checks whether the local player is currently marked as on duty.

##### Return:
- `boolean`: `true` if on duty, `false` otherwise.

> [!NOTE]
> - **ESX**: Performs an asynchronous callback request to the server to verify duty status.
> - **QBox**: Resolves the state immediately by checking the client-cached `job.onduty` value.

##### Example
```lua
local onDuty = TG.Core.GetJobDuty()
```


### TG.Core.VehicleList
----
Returns a sorted catalog list of all vehicles registered in the framework configuration database. Useful for populating vehicle spawn menu options.

##### Return:
- `table`: List array of vehicle selection items `{ label, value }`.

##### Example
```lua
local options = TG.Core.VehicleList()
-- Suitable for use inside TG.menu options
```