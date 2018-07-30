# Material Basics

Materials are the cornerstone of viewing content in 3d. Your scene can have many objects with geometry but if no materials are applied you will not see anything in your viewport. 

Materials get applied to Meshes.  By default materials area specific color (white) but this can be customized in a variety of ways.

```haxe
// creates a cube to act as our geometry
var cube = new h3d.prim.Cube();

// translate it so its center will be at the center of the cube
cube.translate( -0.5, -0.5, -0.5);

//Create a mesh out of our geometry - it has a default material applied.
var mesh = new Mesh(prim, s3d);

//Set the material color to Haxe orange
mesh.material.color.setColor(0xEA8220);
```

The result is our unlit cube.
![Unlit cube](img/h3d/mat_nolights.jpg)


As you can see the results are not terribly impressive. Things start to look better once we start working with lights. Continuing from the same code as above.

```haxe
//Add normals to our geometry so that the lighting can react to the faces
cube.addNormals();

//Add a directional light to our scene
var light = new h3d.scene.DirLight(new h3d.Vector(0.5, 0.5, -0.5), s3d);
```

Our cube is looking a lot better.
![Lit cube](img/h3d/mat_lights.jpg)

## Textures

To really customize our materials though we want to use [textures](https://en.wikipedia.org/wiki/Texture_mapping). To keep it simple, texture allow us to apply custom images to our geometry. Textures are [resources](/documentation/Hxd/Resource-Management.md) that need to be loaded into your Heaps application. Once your textures are loaded they need to be added to materials and then applied to geometry. 

Using the geometry from our previous example lets take a look at how to use textures.

```haxe
// creates a cube to act as our geometry
var cube = new h3d.prim.Cube();

// translate it so its center will be at the center of the cube
cube.translate( -0.5, -0.5, -0.5);

//Add normals and texture coordinates to our cube otherwise texture mapping won't work
cube.addNormals();
cube.addUVs();

//get our image resource and convert it into a texture
var tex = hxd.Res.hxlogo.toTexture();

// create a material with this texture
var mat = h3d.mat.Material.create(tex);

//Create a mesh out of our geometry - and apply the material with the texture we just created to it
var mesh = new Mesh(prim,mat, s3d);

//Add a directional light to our scene
var light = new h3d.scene.DirLight(new h3d.Vector(0.5, 0.5, -0.5), s3d);
```

Here you can see our cube with the loaded texture applied
![Cube with texture](img/h3d/mat_texture.jpg)

## Blend Modes

Materials can have blend modes applied to them to blend with the objects behind them. These are comparable to Photoshop blend modes.

Changing blend modes on a material is straightforward. You simply access the .blendMode property of the material you want to change.

```haxe
mat.blendMode = h2d.BlendMode.Multiply;

//Here are the currently supported blend modes
enum BlendMode {
	None;
	Alpha;
	Add;
	SoftAdd;
	Multiply;
	Erase;
	Screen;
}

```

Our Haxe Cube with BlendMode.Add applied
![Cube with texture](img/h3d/mat_blend.jpg)

### See Also
[Lights](/documentation/h3d/Lights.html)

[Shadows](/documentation/h3d/shadows.html)

[PBR Materials](/documentation/h3d/PBRMaterials.html)

[Material (API)](/api/h3d/mat/Material.html)