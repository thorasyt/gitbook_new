# Client Side
You can easily call all of these functions inside your resource script by doing `TG.spawnVehicle`.

### TG.spawnVehicle.getVehicleProperties
----
Gathers all current cosmetic colors, upgrade mods, damages, neon status, and custom tire configurations of a vehicle.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.

##### Return:
- `table`: A key-value list containing vehicle mod settings.

##### Example
```lua
local props = TG.spawnVehicle.getVehicleProperties(vehicle)
print("Vehicle Plate:", props.plate)
print("Engine Health:", props.engineHealth)
```


### TG.spawnVehicle.setProperties
----
Applies a property state table (such as upgrading engine, changing paint, and breaking doors) to a vehicle.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.
- `props`: (table) The properties table to apply.
- `fixVehicle`: (boolean) Optional flag to repair vehicle damage.

##### Example
```lua
TG.spawnVehicle.setProperties(vehicle, properties, true)
```


### TG.spawnVehicle.getvehicleclass
----
Finds the custom category type name of the vehicle.

##### Parameters:
- `model`: (string | number) Vehicle model.

##### Return:
- `string`: Custom type classification name (`'automobile'`, `'bike'`, `'boat'`, `'heli'`, `'plane'`, `'train'`, `'submarine'`, `'trailer'`).

##### Example
```lua
local typeClass = TG.spawnVehicle.getvehicleclass('adder')
```
