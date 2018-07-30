# PBR Materials

Heaps has support for PBR (Physically Based Rendering) Materials. PBR Materials attempt to simulate lighting conditions more accurately than the default material types. PBR materials combine with your environment, lighting and renderer to create realistic looking materials. The material look is derived from 3 properties - the material color, roughness and metallic. Those properties combined with modifications to the environment and lighting power and renderer exposure are what creates a stunning and realistic look.

*Note that PBR Materials are currently supported in HashLink and Browsers supporting WebGL 2.0 or later.

Let's dive into how to set up PBR Materials in heaps. Before we create our Heaps 3D scene we need to tell the engine that we want to use PBR Materials.

```haxe
//The following will automatically create a PBR Material renderer. 
//It will also create the correct environment and 
//lighting system that must be used for PBR materials.
h3d.mat.MaterialSetup.current = new h3d.mat.PbrMaterialSetup();
```

Now we can start setting up our scene to work with PBR Materials. The first step is to create an environment map. PBR Materials don't explicitly need an 
environment map to function but creating one for this example will best showcase the realism of the materials.  The Heaps [PBR Example](samples/pbr.html) has a good environment map to use.

```haxe
//Create a sphere that will be used for our Sky Sphere and the mesh of our PBR materials
var sp = new h3d.prim.Sphere(1, 128, 128);
//Make sure to add UVs and 
sp.addNormals();
sp.addUVs();

//Create a background mesh 
var bg = new h3d.scene.Mesh(sp, s3d);
bg.scale(10);
//Make sure it is always rendered
bg.material.mainPass.culling = Front;
bg.material.mainPass.setPassName("overlay");

//Create an environment map texture
var envMap = new h3d.mat.Texture(512, 512, [Cube]);		
envMap.name = "envMap";
inline function set(face:Int, res:hxd.res.Image) {
    var pix = res.getPixels();
    envMap.uploadPixels(pix, 0, face);
}
//Set the faces for the environment cube map
set(0, hxd.Res.front);
set(1, hxd.Res.back);
set(2, hxd.Res.right);
set(3, hxd.Res.left);
set(4, hxd.Res.top);
set(5, hxd.Res.bottom);

//Create a new environment that we can use to control some of the material behavior
var env = new h3d.scene.pbr.Environment(envMap);
env.compute();

//Set the environment on the custom PBR renderer
renderer = cast(s3d.renderer, h3d.scene.pbr.Renderer);
renderer.env = env;

//Finally create a shader and apply it to the background mesh so we can actually render our environment on screen.   
var cubeShader = bg.material.mainPass.addShader(new h3d.shader.pbr.CubeLod(env.env));
```

At this point your scene should just have a Sky Sphere with nothing else in it but it is now ready to start working with PBR materials. Thankfully there's not too much else to do to get working with PBR Materials. The first step is to create a light.  PBR Materials use custom lights so you need to make sure that you use lights from the 'h3d.scene.pbr' package.

```haxe
//Create a point light - note that it's from the PBR package.
var light = new h3d.scene.pbr.PointLight(s3d);
light.setPos(30, 10, 40);
light.range = 100; 
//Changing the power of the light will affect the look for your PBR materials.
light.power = 2;
```

Now that is done we are ready to actually render an object in our scene using PBR materials. For this example we will create a sphere and assign it some PBR material values.

```haxe
//Create a sphere using the same geometry from the Sky Sphere
var sphere = new h3d.scene.Mesh(sp, s3d);

//Create a shader to store our PBR values - note the first 2 parameters are 'metalness' and 'roughness'
var pbrValues = new h3d.shader.pbr.PropsValues(1.0,0.5,0);

//Assign the values to our sphere's materials
sphere.material.mainPass.addShader(pbrValues);
```

We've finally got something rendering with PBR materials! Your scene should look something like this.

![Initial PBR Sphere](img/h3d/pbr_1.jpg)

Now we can play with the values to get different looks. Lets reduce the roughness to 0.

```haxe
pbrValues.roughness = 0;
```

Now you can see that with no roughness the looks like it is made out of chrome.
![PBR Sphere with no roughness](img/h3d/pbr_norough.jpg)

Let's now bring down the metallic to 0 as well
```haxe
pbrValues.metalness = 0;
```

Now you can see that with no roughness and no metallic the sphere looks like an extremely shiny piece of plastic.
![PBR Sphere with no roughness](img/h3d/pbr_norough_nometal.jpg)

As you can tell simply by adjusting the metalness and roughness settings on the PBR Material you can dramatically change the look of your mesh.

## Exposure

If you want to change the look of your materials you can also play with the exposure on the renderer.  The PBR renderer defaults to an exposure of 0 and reasonable values are between -3 and 3. Note that changing the exposure on your renderer will affect all of the materials in your scene.

```haxe
renderer = cast(s3d.renderer, h3d.scene.pbr.Renderer);
//Brighten up our PBR materials - this should not affect the environment
renderer.exposure = 1.5; 
```





