# Scenes
In the precedent sections we already used `s2d` which represents the active current scene rendered by the `hxd.App`. A **Scene** (represented by [h2d.Scene](https://heaps.io/api/h2d/Scene.html)) represents any scene the user can visit in the Heaps app. Scenes may be an intro showing the developers logo on start-up, the main menu of the game, maybe a cutscene and finally the level or world where the actual game takes place and the player walks around. `h2d.Scene` (represented by [h2d.Scene](https://heaps.io/api/h2d/Scene.html)) is a special object (it is actually a `h2d.Object`, too!) which is at the root of the scene tree. In [hxd.App](https://heaps.io/api/hxd/App.html) it is accessible by using the `s2d` variable. You will need to add your objects to the scene before they can be displayed. The Scene also handles events such as clicks, touch, and keyboard keys.

```haxe
class Myapp extends hxd.App
{
    override private function init():Void
    {
        super.init();
        s2d;                            // the current scene
        var myscene = new h2d.Scene();  // create a new scene
        setScene(myscene);              // set it as the current scene
        s2d;                            // is now newscene

        var myobj2 = new h2d.Object();
        s2d.addChild(myobj2);

        var myobj = new h2d.Object(s2d);// add myobj to s2d by passing s2d as parameter

    }
}
```

## Scenes and camera

Scenes and camera is explained here.

### Sizing

Scenes sizing is explained here.

### Zoom

Camera zoom is explained here.
