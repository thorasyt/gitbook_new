# Client Side
You can easily call all of these functions inside your resource script by doing `TG.callback`.

### TG.callback.register
----
Registers a client-side callback that the server can invoke.

##### Parameters:
- `event`: (string) Unique identifier for the callback.
- `cb`: (function) Callback implementation returning values back to the server.

##### Example
```lua
TG.callback.register('my_resource:client:getData', function(arg1)
    -- Perform operations
    return { data = "Some Client Data" }
end)
```


### TG.callback.run
----
Asynchronously triggers a server callback, passing the response values to a handler function once returned.

##### Parameters:
- `event`: (string) Target server-side callback name.
- `cb`: (function) Response handler callback function.
- `...`: (any) Arguments to pass to the server callback.

##### Example
```lua
TG.callback.run('my_resource:server:queryDatabase', function(result)
    print("Received query result:", json.encode(result))
end, param1)
```


### TG.callback.await
----
Synchronously triggers a server callback, blocking the current thread until the server responds or a 15-second timeout occurs.

##### Parameters:
- `event`: (string) Target server-side callback name.
- `...`: (any) Arguments to pass to the server.

##### Return:
- `...any`: Unpacked response values returned from the server.

##### Example
```lua
local result = TG.callback.await('my_resource:server:queryDatabase', param1)
```
