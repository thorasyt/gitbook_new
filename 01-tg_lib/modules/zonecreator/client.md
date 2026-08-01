# Client Side
You can easily call all of these functions inside your resource script by doing `TG.zones`.

### TG.zones.cylinder
----
Creates a cylindrical zone boundary.

##### Parameters:
- `data`: (table) Config properties:
  - `coords` or `center`: (vector3) Map center coordinates.
  - `radius`: (number) Cylinder radius.
  - `height`: (number) Cylinder height.
  - `onEnter`: (function) Triggers once when player enters the boundary.
  - `onExit`: (function) Triggers once when player exits the boundary.
  - `inside`: (function) Loops execution when inside.
  - `debug`: (boolean) Renders visual guide marker boundaries.

##### Return:
- `table`: The registered `zone` object.

##### Example
```lua
local cylinderZone = TG.zones.cylinder({
    coords = vector3(0.0, 0.0, 70.0),
    radius = 5.0,
    height = 6.0,
    onEnter = function(self)
        print("Entered Cylinder")
    end,
    onExit = function(self)
        print("Exited Cylinder")
    end
})
```


### TG.zones.sphere
----
Creates a spherical zone boundary.

##### Parameters:
- `data`: (table) Config properties:
  - `coords` or `center`: (vector3) Map center coordinates.
  - `radius`: (number) Sphere radius.
  - `onEnter`: (function) Triggers once when player enters the boundary.

##### Return:
- `table`: The registered `zone` object.

##### Example
```lua
local sphereZone = TG.zones.sphere({
    coords = vector3(0.0, 0.0, 70.0),
    radius = 5.0,
    onEnter = function(self)
        print("Entered Sphere")
    end
})
```


### TG.zones.box
----
Creates a box (cuboid) zone boundary.

##### Parameters:
- `data`: (table) Config properties:
  - `coords` or `center`: (vector3) Map center coordinates.
  - `size`: (vector3) Box dimensions `vector3(length, width, height)`.
  - `heading`: (number) Box heading angle rotation.
  - `onEnter`: (function) Triggers once when player enters the boundary.

##### Return:
- `table`: The registered `zone` object.

##### Example
```lua
local boxZone = TG.zones.box({
    coords = vector3(0.0, 0.0, 70.0),
    size = vector3(4.0, 4.0, 4.0),
    heading = 90.0,
    onEnter = function(self)
        print("Entered Box")
    end
})
```


### TG.zones.poly
----
Creates a multi-point boundary polygon.

##### Parameters:
- `data`: (table) Config properties:
  - `points`: (array) List of `vector2` map coordinates outlining the polygon wall.
  - `minZ`: (number) Minimum height boundary.
  - `maxZ`: (number) Maximum height boundary.
  - `onEnter`: (function) Triggers once when player enters the boundary.

##### Return:
- `table`: The registered `zone` object.

##### Example
```lua
local polyZone = TG.zones.poly({
    points = {
        vector2(100.0, 200.0),
        vector2(150.0, 200.0),
        vector2(150.0, 250.0),
        vector2(100.0, 250.0)
    },
    minZ = 20.0,
    maxZ = 40.0,
    onEnter = function(self)
        print("Entered Polygon")
    end
})
```


### zone:remove
----
Deletes the boundary zone and removes it from collision detection.

##### Example
```lua
zone:remove()
```


### zone:contains
----
Checks whether a specific vector coordinate lies within the boundary limits.

##### Parameters:
- `coords`: (vector3) Map coordinates to verify.

##### Return:
- `boolean`: `true` if inside, `false` otherwise.

##### Example
```lua
local isInside = zone:contains(GetEntityCoords(PlayerPedId()))
```
