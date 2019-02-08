DomKit is a library that can be used together with Heaps to create complex UI and integrate custom 2D components into it.

You should get install it first using: `haxelib git domkit https://github.com/ncannasse/domkit.git`

You can then add it to your project libraries with `-lib domkit` (together with `-lib heaps`)

You can see compile and run the corresponding [Heaps Sample](https://github.com/HeapsIO/heaps/blob/master/samples/Domkit.hx)

# Using Heaps Components

In order to use Domkit to create a heaps components, you simply need to implements `h2d.domkit.Object` and define your document in the `SRC` static as the following sample shows:

```haxe
class SampleView extends h2d.Flow implements h2d.domkit.Object {

    static var SRC = 
        <flow class="box" vertical> 
            Hello World! 
            <bitmap src={tile} id="mybmp"/>
        </flow>

    public function new(tile,?parent) {
        super(parent);
        initComponent();
    }
}

...

var view = new SampleView(h2d.Tile.fromColor(0xFF,32,32),s2d);
view.mybmp.alpha = 0.8;
```

You can read more about Domkit Markup Syntax in the reference below.

# Applying CSS

You can then apply CSS at runtime to your document by using the following code:

```haxe
var style = new h2d.domkit.Style();
style.load(hxd.Res.style); // resource referencing  res/style.css (see Heaps Resources documentation)
style.applyTo(view);
```

Here's an example CSS that can apply to previous view:

```css
.box {
    padding : 20;
    background : #400;
}
```

## Runtime CSS reload

In order to get runtime CSS reload you need to:
* use a `hxd.Res.initLocal()` so you use a local filesystem to load resources. This is only available to some platforms such as HashLink
* enable resources live update with `hxd.Res.LIVE_UPDATE = true;`

Then every time you modify your CSS file, the style will be reapplied to all your currently displayed components. Errors will be displayed in case of invalid CSS property or wrongly formatted value.

# Defining custom Components

In order to define custom components, you need to:
* add `@:uiComp("name")` metadata
* implements `h2d.domkit.Object` (if not already inherited from your superclass)
* add `@:p` for each property you wish to expose to domkit markup/CSS

Here's a small example:
```haxe

// MyComp.hx

enum CustomStyle {
    Big;
    Small;
    Medium;
}

@:uiComp("my")
class MyComp extends h2d.Flow implements h2d.domkit.Object {

    @:p public var style(default,set) : CustomStyle;

    function set_style(s) {
        this.style = s;
        // ....
        return s;    
    }

}
```

It is now possible to use your component from any document `SRC` by doing the following:

```
<my style="medium"/>
```

## Custom components with SRC

It is possible to have a component that also have a SRC. In that case, you need to reference the component name in your SRC root node name so its properties can be applied correctly.

## Components resolution

Components are resolved by name by adding `Comp` to the capitalized component name. For instance `<something/>` will try to load `SomethingComp` class from local context.

If you wish to customize this name resolution, you can use an init macro such as:

```haxe
// add --macro Init.setup() to your HXML / Haxe compilation parameters
class Init {
    public static function setup() {
        domkit.Macros.registerComponentsPath("my.comps.$");
    }
}
```

In the path, the `$` character is the capitalized component name. 
By default `"$Comp"` is a registered path in Heaps.

## Custom CSS parsing

Some properties requires custom parsing. For instance color codes, padding boxes, etc.
You can specify which parser method to use by changing `@:p` metadata in the following way:

```haxe
// will use parseTile CSS parser method
@:p(tile) public var tile : h2d.Tile;
``` 

You can use any identifier that is allowed in the current CSS parser. The default Heaps parser can be found in `h2d/domkit/BaseComponents.hx`

You can extend this parser with your own custom parser to support additional CSS parsing rules.
Here's an example:

```
class MyParser extends h2d.domkit.BaseComponents.CustomParser {
    public function parseIntValues( value : domkit.CssValue ) : Array<Int> {
        return switch( value ) {
        case VList(vl): [for( v in vl ) parseInt(v)]; // comma separated values
        default: [parseInt(value)]; // single value
        }
    }
}
```

And then in your component you need to specify which custom parser to use:

```
@:uiComp("my") @:parser(MyParser)
class MyComp extends h2d.Flow implements h2d.domkit.Object {
   ...
   @:p(intValues) public var values : Array<Int>;
}
```

And it means you can now use values this way in attributes or CSS:
```
<my values="1,2,3"/>
```

# Domkit Markup Reference

Domkit markup allows the following syntaxes.

```jsx
<node attr="value"/>
```
A component with a CSS attribute value. Please note that the value has to be valid CSS and is parsed then interpreted based on the components custom parsing rules in order to be translated to the actual runtime value for the corresponding node property.

```jsx
<node attr={expr}/>
```
Set the attribute based on an Haxe code expression. Unlike previous syntax, here the expression must directly evaluates to the property runtime value, without going through CSS interpretation.

```jsx
<node attr/>
```
A shortcut for `attr="true"`. Allows to easily set boolean attributes

```jsx
<node id="identifier"/>
```
Creates a field on the current class and set it to the component value upon initialization.

```jsx
<node id="group[]"/>
```
Creates an Array of components on the current class and push the component value into it upon initialization.

```jsx
<node(value1,value2,value3)/>
```
Allows to pass constructor arguments to your custom component. ATM only Haxe values are allowed to be passed, DomKit does not allow passing CSS values to constructor arguments.

```jsx
<flow>
${ for( t in tiles ) <bitmap src={t} id="btns[]"/> }
</flow>
```
When using `${..}` syntax, you can inject any Haxe code into the markup language, allowing you to add if/for/method calls/etc.

```jsx
Some Text
```
This is the equivalent of `<text text={"Some Text"}/>`, so you can apply CSS to the `text` components created this way.

```jsx
<node>$component</node>
```
When just a Haxe variable identifier is injected into the document content, we assume it contains another component value to be inserted there.


```jsx
@something

@call(value)

@custom.path(value1,value2,value3)
```
These are markup macros that can be processed in a custom manner by setting `domkit.Macros.processMacro` in an init macro. It allows you to process identifier path and arguments (Haxe expressions) to return some markup syntax that will be then processed.
 
# Heaps CSS reference

This is the complete documentation for allowed CSS attributes for native Heaps components.

## object

## drawable

## bitmap

## text

## flow

## Compile-time CSS file type-checking


