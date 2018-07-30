# GPU Particles

Heaps supports rendering particles on the GPU. This allows for an extremely high amount of particles to be rendered on screen with very little performance impact.

Stting up your GPU particles involves first creating a particle system.

```haxe
//Create a particle system and pass it our 3d scene
var particles = new h3d.parts.GpuParticles(s3d); 
```

From there you add particle groups to the system. Each group is it's own separate bundle of particles that work independently. A particle system can support multiple groups.

```haxe
//Create a particle group with a reference to our particle system
var particleGroup = new h3d.parts.GpuParticles.GpuPartGroup(particles);
```

When you are ready to see your particle group on screen you just need to add it to the system.
```haxe
particles.add(particleGroup);
```

## Customizing Particles

GPU Particles have a lot of properties that can be modified to give your particles different behavior.  See the [API](api/h3d/parts/GpuPartGroup.html) docs for a full list.  

```haxe
//The shape at which the particles will be emitted each ar of type GPUEmitMode
particleGroup.emitMode = Cone;

//The angle which the paricles will be emitted
particleGroup.emitAngle = 0.5;

//The distance from the initial spawn location of each particle
particleGroup.emitDist = 0;

//Fade in and fade out time for each particle
particleGroup.fadeIn = 0.8;
particleGroup.fadeOut = 0.8;
particleGroup.fadePower = 10;

//How much the particles are effective by gravity
particleGroup.gravity = 5;

//The initial size of each particle
particleGroup.size = 0.1;
//Random size offset of each particle
particleGroup.sizeRand = 0.5;

//How fast the particles will rotate
particleGroup.rotSpeed = 10;

//The speed of each particle
particleGroup.speed = 2;

//Random variation in speed for each particle
particleGroup.speedRand = 0.5;

//The lifespan of the particle in seconds
particleGroup.life = 2;

//Random variance in lifespan
particleGroup.lifeRand = 0.5;

//The number of particles in a group - these all get uploaded to the GPU
particleGroup.nparts = 10000;

//You can assign a texture to the particle group
//Every particle will have the texture appllied.
particleGroup.texture = hxd.Res.hxlogo.toTexture();

//Use Texture.fromColor to set the color of each particle.
particleGroup.texture = h3d.mat.Texture.fromColor(0xEA8220);

```

10000 textured particles running on the GPU
![Haxe Logo particle group](img/h3d/particles.jpg)

## Looping

By default the particle groups will loop. You can disable this with the following

```haxe
particleGroup.emitLoop = false;
```

If your particle groups are not set to looping you can listen for a completion event to know when the particle system is done emitting. Note that 

```haxe
particles.onEnd = function(){
    //If none of the groups in this particle system are set to looping
    //this method will fire once the system is done emitting particles
}
```
