# Shadows

Heaps has support for real-time shadows.  Objects can both cast and receive shadows.  This is set at a material level.  In order to for shadows to work properly normals need to be calculated for your geometry.

In the following example you can see a simple scene with a couple of spheres and a floor.  The spheres both cast and receive shadows while the floor only receives them.  For shadows to render correctly your scene will need at least 1 [light](/documentation/h3d/lights.html).

```haxe
//Create a floor plane where we can view our cast shadows
var floor = new h3d.prim.Cube(10, 10, 0.1);
//Make sure se add normals
floor.addNormals();
floor.translate( -5, -5, 0);

var m = new h3d.scene.Mesh(floor, s3d);
//Enabled lighting
m.material.mainPass.enableLights = true;
//Enable receieiving of shadows. Our floor does not need to cast
m.material.receiveShadows = true;

//Greate some sphere geometry for casting and receiving shadows
var sphere = new h3d.prim.Sphere(1, 32, 24);
//Make sure we add normals
sphere.addNormals();

var p = new h3d.scene.Mesh(sphere, s3d);
p.z = 1;
p.material.mainPass.enableLights = true;
//Enable casting and receiving of shadows
p.material.shadows = true;
p.material.color.setColor(Std.random(0x1000000));

var p2 = new h3d.scene.Mesh(sphere, s3d);
p2.z = 2.5;
p2.x = 1;
p2.y = 1;
p2.scale(0.5);
p2.material.mainPass.enableLights = true;
//Enable casting and receiving of shadows
p2.material.shadows = true;
p2.material.color.setColor(Std.random(0x1000000));

//Set up a light otherwise we don't get any shadows
var directionalLight = new h3d.scene.DirLight(new h3d.Vector(-0.3, -0.2, -1), s3d);

//Move our camera to a position to see the shadows
s3d.camera.pos.set(12, 12, 6);

//Setup a Camera Controller to move the camera
new h3d.scene.CameraController(s3d).loadFromCamera();
```

![Shadow Example](img/h3d/shadows.jpg)

# Blurring Shadows

To create a more realistic effect you can blur your shadows.  You do so by accessing the shadow pass from the renderer and setting the amount of passes on the blur object.
 
You can see an example here:

```haxe
var shadow = s3d.renderer.getPass(h3d.pass.ShadowMap);
//Increasing the amount of passes increases the amount of blur applied to the shadow
shadow.blur.passes = 3;
```

An example with shadow.blur.passes = 1;
![Shadow blurring set to 1](img/h3d/shadows_blur_1.jpg)

An example with shadow.blur.passes = 10;
![Shadow blurring set to 10](img/h3d/shadows_blur_10.jpg)