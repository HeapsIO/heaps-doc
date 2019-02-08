DomKit is a library that can be used together with Heaps to create complex UI and integrate custom 2D components into it.

You should get install it first using: `haxelib git domkit https://github.com/ncannasse/domkit.git`

You can see compile and run the corresponding [Heaps Sample](https://github.com/HeapsIO/heaps/blob/master/samples/Domkit.hx)

# Using Heaps Components

In order to use Domkit to create a heaps components, you simply need to implements `h2d.domkit.Object` as the following sample shows:

```haxe
class SampleView extends h2d.Flow implements h2d.domkit.Object {

    static var SRC = 
        <flow vertical> 
            Hello World! 
            <bitmap src={tile}/>
        </flow>

    public function new(tile,?parent) {
        super(parent);
    }
}

...

new SampleView(h2d.Tile.fromColor(0xFF,32,32),s2d);
```

# Defining custom Components

_Work in progress_

## Custom CSS parsing

_Work in progress_

# Heaps CSS reference

This is the complete documentation for allowed CSS attributes for native Heaps components.

## object

## drawable

## bitmap

## text

## flow

