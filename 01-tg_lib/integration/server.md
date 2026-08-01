# Server Side
These third-party resource wrappers are available server-side via `TG`.

---

## 🚑 Ambulance Integration

Integrates with healthcare systems (wasabi_ambulance, brutal_ambulancejob, esx_ambulancejob, qbx_medical) to revive players.

### TG.Ambulance.Revive
----
Revives a target player.

##### Parameters:
- `target`: (number) Player server ID.

##### Example
```lua
TG.Ambulance.Revive(playerId)
```

---

## 👗 Clothing Integration

Wraps clothing shops (illenium-appearance, esx_skin, qb-clothing, rcore_clothing, bl_appearance) to open custom appearance menus.

### TG.clothing
----
Opens the clothing/appearance editor menu for the player.

##### Parameters:
- `src`: (number) Player server ID.

##### Example
```lua
TG.clothing(source)
```

---

## 📦 Inventory Integration

Manages database items, stashes, and items manipulation.

### TG.Inventory.RegisterStash
----
Registers a server-side stash (e.g. for job lockers).

##### Parameters:
- `id`: (string) Unique stash ID.
- `label`: (string) Stash name label.
- `slots`: (number) Total inventory slots.
- `maxWeight`: (number) Total inventory weight.
- `owner`: (string | boolean) Stash owner identifier, or `false`.
- `groups`: (table) Access control groups (e.g. `{ police = 0 }`).
- `coords`: (vector3) Map coords of stash.

##### Example
```lua
TG.Inventory.RegisterStash('police_stash', 'Police Locker', 50, 100000, false, { police = 0 }, coordsVec)
```

### TG.Inventory.getPlayerInventory
----
Retrieves player inventory items list.

##### Parameters:
- `target`: (number) Player server ID.

##### Return:
- `table`: Array of items containing `label`, `value` (item name), `count`, and `img` (image URL).

##### Example
```lua
local items = TG.Inventory.getPlayerInventory(source)
```

### TG.Inventory.additem
----
Adds an item to player inventory.

##### Parameters:
- `target`: (number) Player server ID.
- `item`: (string) Item name.
- `count`: (number) Amount to add.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.
- `any`: Return response object.

##### Example
```lua
local success = TG.Inventory.additem(source, 'water', 2)
```

### TG.Inventory.removeItem
----
Removes an item from player inventory.

##### Parameters:
- `target`: (number) Player server ID.
- `item`: (string) Item name.
- `count`: (number) Amount to remove.

##### Return:
- `boolean`: `true` if successful, `false` otherwise.

##### Example
```lua
local success = TG.Inventory.removeItem(source, 'water', 1)
```

### TG.Inventory.ClearInventory
----
Clears all items from player inventory.

##### Parameters:
- `target`: (number) Player server ID.
- `keep`: (array | string) Optional items names list to preserve.

##### Example
```lua
TG.Inventory.ClearInventory(source, { 'phone' })
```

### TG.Inventory.View
----
Forces open player target inventory on source player screen (admin view).

##### Parameters:
- `src`: (number) Viewer player server ID.
- `action`: (string) Open action name (e.g. `'target'`).
- `target`: (number) Target player server ID.

##### Example
```lua
TG.Inventory.View(source, 'target', targetPlayerId)
```

### TG.Inventory.getItems
----
Retrieves list of all registered inventory items.

##### Return:
- `table`: Array of item listings `{ label, value, img }`.

##### Example
```lua
local itemsList = TG.Inventory.getItems()
```

---

## ♂️ Multicharacter Integration

Integrates with character systems to log out players cleanly.

### TG.Relog
----
Logs out player source back to character selection.

##### Parameters:
- `src`: (number) Player server ID.

##### Example
```lua
TG.Relog(source)
```

---

## ☁️ Weather Sync Integration

Integrates with weather systems (Renewed-Weathersync, easytime, etc.) to set global conditions.

### TG.Weather.SetTime
----
Sets the sync clock time.

##### Parameters:
- `hr`: (number) Hour (0 to 23).
- `min`: (number) Minute (0 to 59).

##### Example
```lua
TG.Weather.SetTime(12, 00)
```

### TG.Weather.SetWeather
----
Sets the sync weather state.

##### Parameters:
- `weather`: (string) Weather type (e.g. `'EXTRASUNNY'`).

##### Example
```lua
TG.Weather.SetWeather('RAIN')
```

### TG.Weather.SetBlackout
----
Sets the global blackout state.

##### Parameters:
- `state`: (boolean) Set to `true` to enable blackout, `false` otherwise.

##### Example
```lua
TG.Weather.SetBlackout(true)
```
