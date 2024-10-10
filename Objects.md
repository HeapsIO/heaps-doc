# Object
Like shown previously in the introduction to give our Heaps application visual (2D) content we need classes that are build upon `h2d.Object`.

- An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *add* to the screen (so to a `h2d.Scene` to be more precise, which mainly is `s2d` when inside `hxd.App`).

- `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.

- However the class itself is *only a **container*** and has nothing to render by itself. For instance `h2d.Flow`, `h2d.Layers` and `h2d.Mask` are all "invisible", too, by that sense. Their job is to *structure in which order* their children objects are rendered. (See more about [[Object trees]])

- A sub-class that *does* rendering however is `h2d.Drawable`. These objects are the base for any actually visible objects in the world of the game like for instance *the player, enemies, buildings, the landscape*, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Text`.
(See more on [[Drawable]])

---
These are all `h2d` package classes (and sub-classes) that extend `h2d.Object` and can be used to create 2D content:
- Console
- [Drawable](Drawable)
  - Anim, Bitmap, Particles, SpriteBatch
  - Graphics
    - Benchmark
  - Interactive
    - Slider
  - [Text](Text)
    - HtmlText, TextInput
  - TileGroup
    - ScaleGrid
  - Video
- [Flow](Flow)
  - CheckBox, Dropdown
- Mask
  - KeyFrames
- [Layers](Layers)
  - CdbLevel, Scene, ZGroup
- ObjectFollower
- Loader


---
## Creating and adding Objects
Objects can be added to a [scene](Scenes) directly (a `h2d.Scene`, for instance `s2d` when inside `hxd.App`) or be added to another `h2d.Object` creating an *object tree*. Object trees are regarded in the next section about [[Object trees]].


```haxe
var a = new h2d.Object();              // object not added yet
s2d.addChild(a);                       // now adding `a` to `s2d` (the currently active 2D scene)

var b = new h2d.Object( s2d );         // `b` is added directly via constructor to `s2d`
b.x = 100;

var c = new h2d.Object( b );           // `c` is added to `b` which has been added to a scene (`s2d`)
c.x = 10;                              // relative to parent `b`

trace( c.getAbsPos().getPosition() );  // the absolute x-position of `c` will be 110, because it "travels" along with its parent `b`
```
