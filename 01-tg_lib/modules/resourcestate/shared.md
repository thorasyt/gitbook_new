# Shared
You can call this function on both the client and server side using `TG.ResourceState`.

### TG.ResourceState
----
Checks whether the specified FiveM resource is currently started on the server/client.

##### Parameters:
- `resourceName`: (string) The name of the resource to check.

##### Return:
- `boolean`: `true` if the resource state is `'started'`, `false` otherwise.

##### Example
```lua
local isStarted = TG.ResourceState('es_extended')
if isStarted then
    print("ESX is running!")
end
```
