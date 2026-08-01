# Shared
You can call this function on both the client and server side using `TG.getNearByPlayers`.

### TG.getNearByPlayers
----
Gets a list of players currently standing within a specific radius of coordinates.

##### Parameters:
- `coords`: (vector3) Map coordinates center.
- `radius`: (number) Distance threshold (default `2.0`).
- `selfInclude`: (boolean) Client-only. Whether to include the local player in the list.

##### Return:
- `table`: Array of tables containing:
  - `id`: (number) Player server ID.
  - `ped`: (number) Player ped entity handle.
  - `coords`: (vector3) Player coordinates.

##### Example
```lua
local nearby = TG.getNearByPlayers(coords, 5.0)
for i = 1, #nearby do
    print("Nearby Player ID:", nearby[i].id)
end
```
