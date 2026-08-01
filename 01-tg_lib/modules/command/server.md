# Server Side
You can easily call all of these functions inside your resource script by doing `TG.commands`.

### TG.commands.add
----
Adds a server-side command listener.

##### Parameters:
- `name`: (string) Command trigger name (e.g. `'givemoney'`).
- `properties`: (table) Command validation and metadata:
  - `restricted`: (string) Required Ace permission node or framework permission node (e.g. `'group.admin'`).
  - `help`: (string) Main description of the command displayed in chat suggestion tooltips.
  - `params`: (array) Parameters validation definition:
    - `name`: (string) Parameter name.
    - `help`: (string) Parameter help text.
    - `optional`: (boolean) Whether the parameter is optional.
    - `type`: (string) `'number'` | `'playerId'` (resolves `'me'` to the executing player source ID and checks if target ID exists).
- `cb`: (function) Callback executed when command runs. Receives `source`, `args` (table of parsed parameters), and `raw` (string).

##### Example
```lua
TG.commands.add('givemoney', {
    restricted = 'group.admin',
    help = 'Give cash to a player',
    params = {
        { name = 'target', help = 'Target player ID (or "me")', optional = false, type = 'playerId' },
        { name = 'amount', help = 'Amount of cash to transfer', optional = false, type = 'number' }
    }
}, function(source, args, raw)
    -- args.target is a validated number representing a player's server ID
    -- args.amount is a casted number
    local success = TG.Core.AddMoney(args.target, 'cash', args.amount)
    if success then
        print(string.format("Gave $%d to player %d", args.amount, args.target))
    end
end)
```
