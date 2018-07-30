# Shaders

You can add a Shader in H2D using `drawable.addShader`.

```haxe
var bmp = new Bitmap(hxd.Res.my_image.toTile());
var shader = new SineDeformShader();
shader.speed = 1;
shader.amplitude = .1;
shader.frequency = .5;
shader.texture = bmp.tile.getTexture();
bmp.addShader(shader);
```

## Creating custom shaders

To create a custom shader, extend `hxsl.Shader` and write the shader source in static `SRC`.
You also need to add `@:import h3d.shader.Base2d;`, which exposes helpful parameters (see next section).

`@:import` will import definitions and thus assume you have this shader as part of your pipeline.

```haxe
class SineDeformShader extends hxsl.Shader {
	static var SRC = {
		@:import h3d.shader.Base2d;
		
		@param var texture : Sampler2D;
		@param var speed : Float;
		@param var frequency : Float;
		@param var amplitude : Float;
		
		function fragment() {
			calculatedUV.y += sin(calculatedUV.y * frequency + time * speed) * amplitude; // wave deform
			pixelColor = texture2D(texture, calculatedUV);
		}
	}
}
```

#### Base 2D values 

For 2D shaders, the following parameters are available in the custom shader. 
When in doubt, can look these up in `h3d.shader.Base2d` (which you have to import).

```haxe
var spritePosition : Vec4;
var absolutePosition : Vec4;
var pixelColor : Vec4;
var textureColor : Vec4;
@var var calculatedUV : Vec2;

var outputPosition : Vec4;
```
