# Server Side
These utility functions are registered and loaded directly during server-side initialization via `init.lua`.

### Utility Functions

### TG.random
----
Generates a randomized alphanumeric string based on a pattern template.

##### Parameters:
- `pattern`: (string) The structure pattern. Supported format characters:
  - `1`: Numbers (`0`-`9`)
  - `A`: Uppercase letters (`A`-`Z`)
  - `a`: Lowercase letters (`a`-`z`)
  - `.`: Alphanumeric characters
- `length`: (number) Optional. Overrides the template string length.

##### Return:
- `string`: The generated random string.

##### Example
```lua
local code = TG.random('11AAaa..', 8)
```


### TG.getNearByPlayers
----
Gets a list of players currently standing within a specific radius of coordinates on the server.

##### Parameters:
- `coords`: (vector3) Map coordinates center.
- `radius`: (number) Distance threshold (default `2.0`).

##### Return:
- `table`: Array of tables containing `id` (player server ID), `ped` (player entity handle), and `coords` (coordinates).

##### Example
```lua
local nearby = TG.getNearByPlayers(coords, 5.0)
for i=1, #nearby do
    print("Nearby Player ID:", nearby[i].id)
end
```


### TG.require
----
A Node-like module searcher and package importer.

##### Parameters:
- `modName`: (string) Module name or path.

##### Return:
- `any`: The exported value of the module.

##### Example
```lua
local ESX = require('@es_extended/imports')
```


### TG.loadJson
----
Loads and decodes a `.json` file from a resource path.

##### Parameters:
- `filePath`: (string) Path to JSON file.

##### Return:
- `table`: Decoded JSON object table.

##### Example
```lua
local config = TG.loadJson('config')
```


### TG.load
----
Loads and runs a lua script file dynamically from a resource path.

##### Parameters:
- `filePath`: (string) Path to script.
- `env`: (table) Optional environment table.

##### Return:
- `any`: Executed script results.

##### Example
```lua
local customModule = TG.load('@my_resource/custom/module')
```


### TG.SaveJson
----
Encodes and saves a JSON data table to a file path.

##### Parameters:
- `filePath`: (string) Destination path.
- `data`: (table) Data table.

##### Example
```lua
TG.SaveJson('settings', { theme = 'dark' })
```


### TG.Debug
----
Pretty-prints tables or message logs into the server console.

##### Parameters:
- `msg`: (any) The object to debug.

##### Example
```lua
TG.Debug(myComplexTable)
```


### TG.GetPlayerName
----
Fetches the display name of a target player ID. On the server side, this resolves the player's Discord nickname or username using the bot API integration.

##### Parameters:
- `id`: (number) Player ID.

##### Return:
- `string`: Player name.

##### Example
```lua
local name = TG.GetPlayerName(playerId)
```
