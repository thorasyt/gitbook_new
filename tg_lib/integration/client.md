# Client Side
These third-party resource wrappers are available client-side via `TG`.

---

## ⛽ Fuel Integration

Automatically detects and wraps fuel resources (LegacyFuel, ps-fuel, cdn_fuel, Renewed-Fuel, ox_fuel, etc.) to set or query vehicle fuel levels.

### TG.fuel.Set (or TG.Fuel.Set)
----
Sets the fuel level of a vehicle.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.
- `amount`: (number) Fuel level (0.0 to 100.0).

##### Example
```lua
TG.fuel.Set(vehicle, 85.0)
```

### TG.fuel.Get (or TG.Fuel.Get)
----
Gets the current fuel level of a vehicle.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.

##### Return:
- `number`: Current fuel level.

##### Example
```lua
local currentFuel = TG.fuel.Get(vehicle)
```

---

## 📦 Inventory Integration

Allows opening stashes or player inventories on the client.

### TG.Inventory.openInventory
----
Opens a target inventory (like `ox_inventory`).

##### Parameters:
- `invType`: (string) The type of inventory (e.g. `'stash'`).
- `data`: (table | string) Target data containing the unique ID or the ID string itself.

##### Example
```lua
TG.Inventory.openInventory('stash', 'police_stash_1')
```

---

## 👁️ Target Integration

Automatically wraps target resources (like `ox_target`).

### TG.Target.addLocalEntity
----
Registers a client-side local entity with the targeting system.

##### Parameters:
- `entity`: (number) Entity handle.
- `options`: (table) Target interaction options.

##### Return:
- `boolean`: `true` if registered successfully, `false` otherwise.

##### Example
```lua
TG.Target.addLocalEntity(ped, {
    {
        name = 'talk_ped',
        icon = 'icon-comments',
        label = 'Talk to NPC',
        onSelect = function()
            print("Interacting with NPC!")
        end
    }
})
```

---

## 🔑 Vehicle Keys Integration

Sets or removes vehicle lock ownership for active key resources (qb-vehiclekeys, wasabi_carlock, Renewed-Vehiclekeys, qs-vehiclekeys, etc.).

### TG.vehiclekeys.add
----
Grants vehicle key ownership to the client.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.
- `plate`: (string) Vehicle license plate.

##### Example
```lua
TG.vehiclekeys.add(vehicle, 'TGLIB')
```

### TG.vehiclekeys.remove
----
Removes vehicle key ownership.

##### Parameters:
- `vehicle`: (number) Vehicle entity handle.
- `plate`: (string) Vehicle license plate.

##### Example
```lua
TG.vehiclekeys.remove(vehicle, 'TGLIB')
```
