# Server Side
You can easily call all of these functions inside your resource script by doing `TG.database`.

### TG.database.isExist
----
Synchronously executes database queries (`await` mode) and returns query rows, scalar value, or returns `false` if empty.

##### Parameters:
- `type`: (string) Query method: `'single'` | `'query'` | `'insert'` | `'scalar'` | `'update'`.
- `query`: (string) The SQL statement containing placeholders.
- `params`: (table) Bind parameters array.

##### Return:
- `table | number | string | boolean`: Query result if found, `false` otherwise.

##### Example
```lua
local user = TG.database.isExist('single', 'SELECT * FROM players WHERE citizenid = ?', { cid })
if user then
    print("User found:", user.name)
else
    print("User does not exist")
end
```


### TG.database.execute
----
Asynchronously runs SQL queries, passing database outputs back to a callback once complete.

##### Parameters:
- `type`: (string) Query method: `'prepare'` | `'query'` | `'insert'` | `'update'`.
- `query`: (string) The SQL statement containing placeholders.
- `params`: (table) Bind parameters array.
- `cb`: (function) Asynchronous callback function receiving the DB response.

##### Example
```lua
TG.database.execute('query', 'SELECT * FROM characters LIMIT 5', {}, function(rows)
    if rows then
        for i = 1, #rows do 
            print(rows[i].charname) 
        end
    end
end)
```


### TG.database.insertData
----
Alias of [TG.database.execute](server.md#tgdatabaseexecute) for inserting data asynchronously.
