# Server Side
You can easily call all of these functions inside your resource script by doing `TG.callback`.

### TG.callback.register
----
Registers a server-side callback that clients can trigger.

##### Parameters:
- `name`: (string) Unique callback name.
- `cb`: (function) Server handler function returning values to the client. The callback function receives the player's `source` as its first argument.

##### Example
```lua
TG.callback.register('my_resource:server:queryDatabase', function(source, param1)
    -- Query logic here
    return { status = "Success", id = source }
end)
```


### TG.callback.run
----
Asynchronously triggers a callback on a specific client, sending the result to a callback handler.

##### Parameters:
- `event`: (string) Target client-side callback name.
- `playerId`: (number) Target player's server ID.
- `cb`: (function) Response handler callback function.
- `...`: (any) Arguments to pass to the client.

##### Example
```lua
TG.callback('my_resource:client:getData', playerId, function(clientData)
    print("Player data received:", clientData)
end, arg1)
```


### TG.callback.await
----
Synchronously triggers a callback on a specific client, waiting for a return value.

##### Parameters:
- `event`: (string) Target client-side callback name.
- `playerId`: (number) Target player's server ID.
- `...`: (any) Arguments to pass to the client.

##### Return:
- `...any`: Unpacked response values returned from the client.

##### Example
```lua
local clientData = TG.callback.await('my_resource:client:getData', playerId, arg1)
```
