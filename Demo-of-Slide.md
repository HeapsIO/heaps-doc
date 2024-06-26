# Demo of Slide

This is a short demo of how [slide](https://lib.haxe.org/p/slide) can be implemented in Heaps.
Slide is described as a framework agnostic *tweening* library. In the sample below we use it
to give our h2d.Object (a `h2d.Graphics` finally) a predefined path to move along.

Slide can be installed with:
`haxelib install slide`
or
`haxelib git slide https://github.com/AndreiRudenko/slide.git`

## Preview

![demo_of_slide](https://user-images.githubusercontent.com/88530062/178114434-17d25386-aad3-408a-9e09-ab686c56cc32.png)


## Code

```haxe

import slide.Slide;

class SlideLibDemo extends hxd.App {
    static function main() { new SlideLibDemo(); }
    override function init() {
        var g = new h2d.Graphics( s2d );
        g.beginFill( 0xFFFFFF );
        g.drawRect( 0, 0, 50, 50 );
        g.setPosition(60,60);

        var foo = ()->{ trace("Cycle accomplished!"); };
        var fin = ()->{ trace("Slide finished."); };

        Slide.tween( g )
            .to({x:400}, 1.5)
            .to({y:300}, 1  )
            .wait(0.5)
            .to({x:60,y:60}, 0.5)
            .ease(slide.easing.Quad.easeIn)
            .call( (o)->{ foo(); trace(o,o.x,o.y); o.rotate(0.3); } )
            .onComplete( fin )
            .repeat(-1) // -1 = infinity (actually prevents function fin to be called here)
            .start();
    }

    override function update(dt:Float) {
        Slide.step(dt); // tell Slide how many seconds have elapsed
    }
}
```

Note that in the end we pass our update method's `dt` parameter on to Slide, thus the latter knows how many time has passed and can adapt it's tweens accordingly.

## hxml-file sample when using HashLink

```
-lib heaps
-lib hlsdl
-lib slide
-main SlideLibDemo
-hl slide_demo.hl
```