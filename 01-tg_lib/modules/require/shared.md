# Shared
These functions manage script loading, environment isolation, and JSON file storage on both client and server side.

### require (Global Replacement)
----
Replaces the global `require` function to dynamically load FiveM resources using a Node.js-style module loader.

##### Parameters:
- `moduleName`: (string) Module name or path.

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
