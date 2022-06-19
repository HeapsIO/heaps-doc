# Object
Like shown previously in the introduction to give our Heaps application visual (2D) content we need classes that are build upon `h2d.Object`.

An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *see* on the screen. Therefore `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.
`h2d.Object`s can be any visual objects in the world of the game like for instance *the player, enemies, buildings*, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Flow` that arranges UI elements and many more.

These are all `h2d` package classes (and sub-classes) that extend `h2d.Object` and can be used to create 2D content:
- Console
- Drawable
  - Anim, Bitmap, Particles, SpriteBatch
  - Graphics
    - Benchmark
  - Interactive
    - Slider
  - Text
    - HtmlText, TextInput
  - TileGroup
    - ScaleGrid
  - Video
- Flow
  - CheckBox, Dropdown
- Mask
  - KeyFrames
- Layers
  - CdbLevel, Scene, ZGroup
- ObjectFollower
- Loader

---
## Object trees
Objects can be added to a scene directly (a `h2d.Scene`, for instance `s2d` when inside `hxd.App`) or be added to another `h2d.Object` creating an *object tree*. Object trees are regarded in the next section about [[Object trees]].
