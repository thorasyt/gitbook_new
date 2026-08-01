# Server Side
You can easily call this function inside your resource script by doing `TG.random`.

### TG.random
----
Generates a randomized alphanumeric string based on a pattern template.

##### Parameters:
- `pattern`: (string) The structure pattern. Supported format characters:
  - `1`: Numbers (`0`-`9`)
  - `A`: Uppercase letters (`A`-`Z`)
  - `a`: Lowercase letters (`a`-`z`)
  - `.`: Alphanumeric characters (either uppercase letter or number)
- `length`: (number) Optional. Overrides the pattern length (either padding with spaces or omitting characters).

##### Return:
- `string`: The generated random string.

##### Example
```lua
local code = TG.random('11AAaa..', 8)
```
