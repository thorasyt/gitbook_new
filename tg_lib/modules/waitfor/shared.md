# Shared
You can call this function on both the client and server side using `TG.waitFor`.

### TG.waitFor
----
Blocks execution of the current thread until the callback function returns a non-nil value. Throws an error on timeout.

##### Parameters:
- `cb`: (function) The condition check callback. Must return a non-nil value to resolve.
- `errMessage`: (string) Optional. Error message to print if the timeout occurs.
- `timeout`: (number) Optional. The timeout threshold in milliseconds (default `1000`ms).

##### Return:
- `any`: The non-nil value returned by `cb()`.

##### Example
```lua
-- Wait for entity to exist before proceeding
local ped = TG.waitFor(function()
    local playerPed = PlayerPedId()
    if playerPed and playerPed ~= 0 then
        return playerPed
    end
end, "Player ped failed to load", 5000)
```
