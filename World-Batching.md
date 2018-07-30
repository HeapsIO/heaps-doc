# World Batching

Because modern 3D scenes can often get large, certain optimizations need to be made to keep your application running at an acceptable framerate. Once of these optimizations involves batching. Heaps offers a way to batch meshes into a single object that will reduce the amount of draw calls and keep your frame rate high.

To get started you first need to create a world object.
```haxe
//The world will be divided in this many "chunks"
var numberOfChunks = 64;
//How big our world will be in units
var worldSize = 256;

//Create our world with the supplied values.
var world = new h3d.scene.World(numberOfChunks, worldSize, s3d);
```

Now we need to add objects to our world. Objects need to be loaded directly via the world object so certain batching optimizations can take place. You then reuses these obejcts when you want to disperse them through your world.

```haxe
//Loud our resources from the world
//These are taken from the World sample application
var tree = world.loadModel(hxd.Res.tree);
var rock = world.loadModel(hxd.Res.rock);

//Add 5000 objeects to our scene - randomly choosing between a rock and a tree
for( i in 0...5000 )
			world.add(  Std.random(2) == 0 ? tree : rock,
                        Math.random() * worldSize, 
                        Math.random() * worldSize, 
                        0, 
                        1.2 + hxd.Math.srand(0.4), 
                        hxd.Math.srand(Math.PI)
                    );

//Let our world know that we are done batching items so it can perform some optimizations
world.done();

//Add some lighting to that our scene looks good
new h3d.scene.DirLight(new h3d.Vector( 0.3, -0.4, -0.9), s3d);
s3d.lightSystem.ambientLight.setColor(0x909090);

//Adjust the camera settings and create a controller so we can observer our scene
s3d.camera.target.set(72, 72, 0);
s3d.camera.pos.set(120, 120, 40);
s3d.camera.zNear = 1;
s3d.camera.zFar = 100;
new h3d.scene.CameraController(s3d).loadFromCamera();
```

You should now see something like the following. If you rotate your camera you should see no significant slowdown in FPS even with 5000 objects in your scene.
![Batched world running at high FPS](img/h3d/world_batching.jpg)