# Scenes
In the precedent sections we already used `s2d` which represents the active current scene rendered by the `hxd.App`.

A **Scene** (represented by [h2d.Scene](https://heaps.io/api/h2d/Scene.html)) represents any scene the user can visit in the Heaps app. Scenes may be *an intro showing the developers logo on start-up*, the *main menu of the game*, maybe a *cutscene* and finally *the level or world* where the actual game takes place and the player walks around.

`h2d.Scene` (represented by [h2d.Scene](https://heaps.io/api/h2d/Scene.html)) is a special object (it is actually a `h2d.Object`, too!) which is at the root of the scene tree. In [hxd.App](https://heaps.io/api/hxd/App.html) it is accessible by using the `s2d` variable (as we have seen previously). You will need to add your objects to the scene before they can be displayed. The Scene also handles events such as clicks, touch, and keyboard keys.

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

public static function main(){ new Myapp(); }
}
```

## Use of scenes

You can deal with scenes mostly any way you like (like with most things in Heaps!) and just make changes to the `s2d` variable directly. However to give a better understanding what scenes are see the following sample application. When running you will see a very basic intro scene, a main menu and the actual game scene Level01. Using each a class for each scene may look a bit bloated, but could give a starting point how you *can* structure your scenes as single files in your project.

### Demo how they can be used

![demo_on_using_scenes](https://user-images.githubusercontent.com/88530062/174429682-6debdb8e-d35c-4f3e-87d0-d3d72fe7e2b4.png)

```haxe
class ABC extends hxd.App {
    public static var app : ABC;                // just will represent this app as an individual object at runtime (so as an instance of this class)
    static function main() { app = new ABC(); } // "save" the reference for access from outside later
    public var myUpdateFunction : Dynamic;      // this will allow us to define the update function from other classes (see Level01)
    public function new() {super();}
    override function init() {
        setScene( new Intro() );
    }
    override function update(dt:Float) {
        if(myUpdateFunction!=null)
            myUpdateFunction();
    }
}

class Intro extends h2d.Scene {
    public function new() {
        super();
        var t = new h2d.Text( hxd.res.DefaultFont.get(), this ); t.text = "Intro scene";
        trace(t.text);
        var t = new h2d.Text( hxd.res.DefaultFont.get(), this );
        t.setPosition( 50,  50 );
        t.text = "Game by _____";
        t.scale( 3 ); t.textColor = 0x00FFFF;
        var t = new h2d.Text( hxd.res.DefaultFont.get(), this );
        t.setPosition( 50, 150 );
        t.text = "Created with the Heaps game engine.\nSee more on\nhttps://heaps.io";
        t.scale( 2 ); t.textColor = 0x606060;
        var t = new haxe.Timer( 3 * 1000 );
        t.run = () -> {
            ABC.app.setScene( new MainMenu() );
            t.stop();
        };
    }
}

class MainMenu extends h2d.Scene {
    public function new() {
        super();
        var t = new h2d.Text( hxd.res.DefaultFont.get(), this ); t.text = "Main menu scene";
        trace(t.text);
        // base flow for all elements
        var f = new h2d.Flow( this ); f.verticalSpacing = 8; f.layout = h2d.Flow.FlowLayout.Vertical;
        f.setPosition( 100, 100 );
        // main title
        var mainmenu_title_flow = new h2d.Flow( f );
        var mainmenu_title = new h2d.Text( hxd.res.DefaultFont.get(), mainmenu_title_flow );
        mainmenu_title.text = "Main menu"; mainmenu_title.scale(3); mainmenu_title_flow.padding = 32;
        // buttons
        var button_play = new h2d.Interactive( 300, 50, f ); button_play.backgroundColor = 0xFF00FF80;
        var button_quit = new h2d.Interactive( 300, 40, f ); button_quit.backgroundColor = 0xFFFF4040;
        //button_quit.y += 60;
        button_play.onClick = (e)->{
            ABC.app.setScene( new Level01() );
        };
        button_quit.onClick = (e)->{ hxd.System.exit(); };
        var t = new h2d.Text( hxd.res.DefaultFont.get(), button_play ); t.text = "Play"; t.setPosition(8,8);
        var t = new h2d.Text( hxd.res.DefaultFont.get(), button_quit ); t.text = "Quit"; t.setPosition(8,8);
    }
}

class Level01 extends h2d.Scene {
    public function new() {
        super();
        var t = new h2d.Text( hxd.res.DefaultFont.get(), this ); t.text = "Level01 scene";
        trace(t.text);
        var player = new h2d.Graphics( this ); player.setPosition( 200, 200 );
        drawSmileyFace( player );
        var t = new h2d.Text( hxd.res.DefaultFont.get(), player ); t.text = "Player"; t.setPosition(-50,50);
        ABC.app.myUpdateFunction = () -> { player.x += 0.1; };
    }
    function drawSmileyFace( g:h2d.Graphics ) {
        // face
        g.beginFill( 0xFFFF00 );
        g.drawCircle( 0, 0, 40 );
        // first eye
        g.beginFill( 0x0 );
        g.drawCircle( -20, 0, 7 );
        // second eye
        g.beginFill( 0x0 );
        g.drawCircle( 20, 0, 7 );
        // a happy mouth
        g.beginFill( 0x0 );
        g.drawPie( 0, 10, 15, 0*Math.PI, 1*Math.PI );
        // and some eye sparkle
        g.beginFill( 0xFFFFFF );
        g.drawCircle( -20, -5, 3 );
        g.beginFill( 0xFFFFFF );
        g.drawCircle( 20, -5, 3 );
    }
}
```

---


## Scenes and camera

Scenes and camera is explained here.

### Sizing

Scenes sizing is explained here.

### Zoom

Camera zoom is explained here.
