# Server Side
You can easily call all of these functions inside your resource script by doing `TG`.

### TG.GetStartTime
----
Returns the server uptime string formatted.

##### Return:
- `string`: Uptime duration string formatted as `XXh XXm` (e.g. `"02h 45m"`).

##### Example
```lua
local uptime = TG.GetStartTime()
print("Server Uptime:", uptime)
```


### TG.GetPlayerStatus
----
Returns lists containing history of player connection counts alongside their log times.

##### Return:
- `table`: A dictionary with keys `times` (array of string time stamps) and `players` (array of number counts).

##### Example
```lua
local status = TG.GetPlayerStatus()
-- Result: { times = { "19:00", "19:30" }, players = { 24, 30 } }
for i = 1, #status.times do
    print(string.format("At %s, there were %d players online", status.times[i], status.players[i]))
end
```


### TG.GetServerInfo
----
Returns key server configurations and environment characteristics.

##### Return:
- `table`: A dictionary containing:
  - `serverName`: (string) Hostname of the server (`sv_hostname`).
  - `maxPlayers`: (number) Max clients slots (`sv_maxclients`).
  - `resourceCount`: (number) Total number of resources started/loaded on the server.
  - `uptime`: (string) Calculated uptime duration.
  - `artifact`: (number) Server build artifact version.
  - `onesync`: (string) OneSync status (e.g. `'on'`, `'off'`, `'legacy'`).

##### Example
```lua
local info = TG.GetServerInfo()
print("Max Player Slots:", info.maxPlayers)
print("Total Resources Running:", info.resourceCount)
print("Artifact Version:", info.artifact)
```
