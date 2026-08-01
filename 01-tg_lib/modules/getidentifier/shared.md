# Shared
You can call this function on both the client and server side using `TG.getIdentifier`.

### TG.getIdentifier
----
Retrieves player identifiers for a specific player source.

##### Parameters:
- `source`: (number) The target player's server ID.
- `key`: (string) Optional. The identifier type prefix to search for (e.g. `'license'`, `'steam'`, `'discord'`, `'xbl'`, `'live'`, `'ip'`).

##### Return:
- `string | table | boolean`:
  - If `key` is specified: Returns the identifier value (excluding prefix) as a `string`, or `false` if not found.
  - If `key` is nil: Returns a key-value `table` of all identifiers belonging to the player (e.g. `{ license = "...", discord = "..." }`).
  - Returns `false` if `source` is invalid or identifiers are empty.

##### Example
```lua
-- Retrieve license string specifically
local license = TG.getIdentifier(source, 'license')

-- Retrieve all identifiers
local identifiers = TG.getIdentifier(source)
if identifiers then
    print("Discord ID:", identifiers.discord)
    print("Steam Hex:", identifiers.steam)
end
```
