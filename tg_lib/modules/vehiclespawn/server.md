# Server Side
You can easily call all of these functions inside your resource script by doing `TG.spawnVehicle`.

### TG.spawnVehicle.SetUp
----
Spawns a vehicle server-side, updates plate attributes, assigns routing buckets, and peds placement, verifying state networks before returning.

##### Parameters:
- `model`: (string | number) Vehicle model name (e.g. `'adder'`) or model hash.
- `mtype`: (string) Type of vehicle (e.g. `'automobile'`).
- `source`: (number) The server player ID requesting the spawn.
- `warp`: (boolean) Set to `true` to warp the player into the driver seat.
- `vehicleData`: (table) Target properties like plate string.
  - `plate`: (string) Plate string.
  - `props`: (table) Core mod properties.
- `bucket`: (number) Routing bucket ID (set `0` for default).
- `coords`: (vector4) Target location coordinates including heading.

##### Return:
- `number`: The network ID of the vehicle.
- `number`: The spawned vehicle entity handle.

##### Example
```lua
local netId, vehicle = TG.spawnVehicle.SetUp('adder', 'automobile', source, true, { plate = 'TGLIB' }, 0, vector4(100.0, -200.0, 30.0, 90.0))
```


### TG.spawnVehicle.GetVehicleClass
----
Checks the class of a model from the client side wrapper.

##### Parameters:
- `src`: (number) Invoking player's server ID.
- `model`: (string | number) Vehicle model.

##### Return:
- `string`: Vehicle type (e.g. `'automobile'`, `'bike'`, `'boat'`, `'heli'`, `'plane'`).

##### Example
```lua
local vehicleType = TG.spawnVehicle.GetVehicleClass(source, 'adder')
```
