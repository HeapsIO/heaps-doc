# Filters

Filters apply to the display tree, so can be used to create screen effects.
You can assign a filter to a `h2d.Sprite`. 

```haxe
mySprite.filter = new Glow();
```

## Build-in filters

Heaps provides several filters like Ambient, Bloom, Blur, ColorMatrix, Displacement, DropShadow, Glob, Mask. 
They can be found in the `h2d.filter` package.

## Creating custom screen shader filters

To create a shader that works as filter, you need a screen shader.

As you can see in `h3d.shader.ScreenShader`, screen shaders only provides you `input.position` and `input.uv`, and you can alter the `output.position` or `output.color`.

```haxe
@input var input : {
	position : Vec2,
	uv : Vec2,
};
var output : {
	position : Vec4,
	color : Vec4,
};
```

Let's create a simple shader that changes the red channel based on a parameter:

```haxe
class MyFilterShader extends h3d.shader.ScreenShader {
	static var SRC = {
		@param var red : Float;
		
		function fragment() {
			output.color.r = red; // change red channel
		}
	}
}
```
Usage:

```haxe
var customFilter = new h2d.filter.Shader(new MyFilterShader());
customFilter.red = 0.1; // use 0-1 range
mySprite.filter = customFilter;
```

## Multiple filters on one sprite

To apply multiple filters, use `h2d.filter.Group`.

```haxe
var group = new Group([filter1, filter2]);
mySprite.filter = group;
```