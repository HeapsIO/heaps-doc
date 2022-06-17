### Object
An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *see* on the screen. Therefore `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.
`h2d.Object`s can be any visual objects in the world of the game like for instance the player, enemies, buildings, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Flow` that arranges UI elements and many more.

#### Object trees
Objects can be added to a scene (here `h2d.Scene`) directly or be added to another `h2d.Object` creating an *object tree*.
The child objects will inherit the transformations of the parent object they have been added to. This means whenever the parent transforms (changing the position, rotating, making it transparent or invisible etc.) these transformations *also* apply to all the children that have been added to this parent object.

```haxe
var a = new h2d.Object();              // object not added yet
s2d.addChild(a);                       // now adding `a` to `s2d` (the currently active 2D scene)

var b = new h2d.Object( s2d );         // `b` is added directly via constructor to `s2d`
b.x = 100;

var c = new h2d.Object( b );           // `c` is added to `b` which has been added to a scene (`s2d`)
c.x = 10;                              // relative to parent `b`

trace( c.getAbsPos().getPosition() );  // the absolute x-position of `c` will be 110, because it "travels" along with its parent `b`
```

`h2d.Object` is also documented in detail in the [API](https://heaps.io/api/h2d/Object.html).