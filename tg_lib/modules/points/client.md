# Client Side
You can easily call all of these functions inside your resource script by doing `TG.Points`.

### TG.Points.new
----
Registers a coordinate tracking point on a high-performance spatial grid.

##### Parameters:
- `data`: (table) Config properties:
  - `coords`: (vector3) Map coordinates of the point.
  - `distance`: (number) Activation/interaction radius (default `2.0`).
  - `nearbyDistance`: (number) Proximity threshold (defaults to `distance`).
  - `onEnter`: (function) Triggers once when entering threshold. Receives `self` (point object).
  - `onExit`: (function) Triggers once when leaving threshold. Receives `self` (point object).
  - `nearby`: (function) Loops execution when inside radius. Receives `self` (point object) and `dist` (current distance).
  - `inNearby`: (function) Alternate loop callback executed when inside radius.
  - `debug`: (boolean) Renders visual radius ring and marker.
  - `debugColor`: (table) Debug marker RGB color table `{ r = 255, g = 255, b = 0 }`.

##### Return:
- `table`: The registered `point` object.

##### Example
```lua
local point = TG.Points.new({
    coords = vector3(440.0, -980.0, 30.0),
    distance = 4.0,
    onEnter = function(self)
        print("Walking near point")
    end,
    onExit = function(self)
        print("Leaving point zone")
    end,
    nearby = function(self, distance)
        -- Renders UI 3D Text or prompt
    end
})
```


### point:remove
----
Destroys the tracking point and removes it from spatial checks.

##### Example
```lua
point:remove()
```


### point:setCoords
----
Dynamically updates point location coordinates on the spatial grid.

##### Parameters:
- `coords`: (vector3) The new coordinate location.

##### Example
```lua
point:setCoords(vector3(100.0, 20.0, 10.0))
```


### point:setDistance
----
Dynamically updates point's check distance radius.

##### Parameters:
- `distance`: (number) New proximity distance.

##### Example
```lua
point:setDistance(5.0)
```


### point:isNearby
----
Checks if the player is currently within the point's proximity.

##### Return:
- `boolean`: `true` if nearby, `false` otherwise.

##### Example
```lua
local nearby = point:isNearby()
```


### point:getDistance
----
Calculates current distance from the player to the point coordinates.

##### Return:
- `number`: The distance.

##### Example
```lua
local dist = point:getDistance()
```


### point:pause
----
Toggles active proximity/collision checks on the point.

##### Parameters:
- `paused`: (boolean) Set to `true` to suspend, `false` to resume.

##### Example
```lua
point:pause(true)
```
