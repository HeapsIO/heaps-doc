# Lights 

Heaps supports real-time lighting.  There are currently two types of lights supported, directional and point lights.

# Enabling Lights
In order for lights to be enabled on your objects, you need to specifcy it on an object's material.  You do so as follows:

```haxe
myLitMesh.material.mainPass.enableLights = true;
```

# Point Lights

Point lights are lights that are part of the environment, have a specific color and position and illuminate any materials (light enabled materials that is) around it. 

To create a point light you do the following:

```haxe
//Create the point light by passing it our 3d scene
var myPointLight = new h3d.scene.PointLight(s3d);
//Set the position on the light
myPointLight.x = 10;
myPointLight.y = 10;
myPointLight.z = 50;
//Set the color on the light
myPointLight.color.setColor(0xffffff);
```

# Direction Lights

Directional lights are lights that affect every object with a light enabled material in your scene and like their name implies, emit in a specific direction.  They have no position.  They are comparable to the light from a source like the Sun.  

The following is an example of how to create a directional light

```
//Create the directional light by giving it a Vector indicating the direction in which it illuminates and a reference to our 3d scene
var directionalLight = new h3d.scene.DirLight(new h3d.Vector(0.2, 0.3, -1), s3d);
//Set the color on the directional light
directionalLight.color.set(0.1, 0.1, 0.1);
```
# Specular Lighting

Both directional and point lights support specular lighting.  You can enable specular lighting on your lights with the following:

```haxe
myDirectionalOrPointLight.enableSpecular = true;
```


# Ambient lighting

Ambient lighting is a color that illuminates the entire scene with no specific direction.

You can set up ambient lighting as follows

```haxe
//Set the color on the ambient light object
s3d.lightSystem.ambientLight.set(0, 0, 0);
```

### See Also
[Shadows](/documentation/h3d/shadows.html).