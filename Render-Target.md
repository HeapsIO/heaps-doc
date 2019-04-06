
Render targets can be used to capture a scene to a `Texture` which can be used later.

Render targets for 3d scenes require both a texture to write to, and a depth buffer. Additionally, you must pass the `Target` flag to the texture during creation.

```haxe
var renderTarget = new Texture( engine.width, engine.height, [ Target ] );
renderTarget.depthBuffer = new DepthBuffer( engine.width, engine.height );
```

## Rendering a scene to texture

To render our scene, we can use `pushTarget` and `popTarget` to change what surface we render to. `pushTarget` will push a new render target on to the render stack. `popTarget` will remove the topmost target. This allows you to nest render operations as needed. You can also use `pushTargets` if you wish to render to multiple textures.

```haxe
engine.pushTarget(renderTarget);

engine.clear(0, 1); // Clears the render target texture and depth buffer
s3d.render(e);

engine.popTarget();
```

#### Displaying a render texture in 2d

To display a render texture result in a 2d context, you can simply create a `Tile` out of the render texture. This will update automatically, as the bitmap will continue to reference the underlying texture. It can then be used as a `Bitmap` or in any other place `Tile`s are used.

```haxe
var renderTargetBitmap = new Bitmap(Tile.fromTexture(renderTarget), s2d);
```

#### Displaying a render texture in 3d

This example will require a second scene. One which we'll render to a texture, and another which we will display it in.

```haxe
override function init() {
	renderTarget = new Texture(engine.width,engine.height, [ Target ]);
	renderTarget.depthBuffer = new DepthBuffer(engine.width, engine.height);

	s3dTarget = new h3d.scene.Scene();

	// The object we're going to display in our scene
	var prim = h3d.prim.Cube.defaultUnitCube();
	var mat = h3d.mat.Material.create(renderTarget);	
	obj1 = new h3d.scene.Mesh(prim, mat, s3d);

	// The object we're going to capture in our Render Target
	obj2 = cache.loadModel(hxd.Res.mdl.test2);
	
	s3dTarget.addChild(obj2);
}

override function render(e:h3d.Engine) {
	// Render the target first
	engine.pushTarget(renderTarget);

	engine.clear(0, 1);
	s3dTarget.render(e);
	
	engine.popTarget();

	// Now render our scene
	s3d.render(e);

	s2d.render(e);   
}
```