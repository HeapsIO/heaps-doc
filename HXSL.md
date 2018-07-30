# Hxsl

## HXSL stands for "Haxe Shader Language"

It's a shader language that follows entirely the OpenGL shader language ([GLSL](https://en.wikipedia.org/wiki/OpenGL_Shading_Language)) specification, but which uses Haxe syntax, allowing you to write directly your shader in your Haxe source code.

Vertex shaders are used for transforming and projecting each geometry point into 2D space and set up "variables" that will be interpolated on a per-pixel basis. This can be used by the pixel shader.

Fragment shaders are used for blending different textures and colors into a single pixel color that will be written to screen.

Example:

```haxe
class MyShader extends hxsl.Shader {
    static var SRC = {
        @input var input : { position : Vec3, normal : Vec3 };
        var output : { position : Vec4, normal : Vec3, color : Vec4 };
        var transformedNormal : Vec3;
        @param var materialColor : Vec4;
        @param var transformMatrix : Mat4;
        function vertex() {
            output.position = vec4(input.position,1.) * transformMatrix;
            transformedNormal = normalize(input.normal * mat3(transformMatrix));
        }
        function fragment() {
            output.color = materialColor;
            output.normal = transformedNormal;
        }
    };
}
```

This shader defines:

* an `input` variable (tagged with `@input` metadata) which lists the vertex properties that will be accessible in the shader (_vertex attributes_ in GLSL)
* several `@param` variables which are per-shader-instance values that will be set for all vertexes (_uniforms_ in GLSL)
* several variables which can be read/written in shader, here we use `output` as a name to store each shader stage result and `transformedNormal` as the value of the vertex normal in absolute world space.

## Variable kind

There are several type of variables in HXSL:

- `@input` are vertex attributes input values
- `@param` are per-shader-instance shader parameters / uniforms
- `@global` are defined for multiple shaders
- `@const` are constants that will produce a different shader output if changed
- `@var` are _varying_ variables that are written in vertex shader and read in pixel shader (optional, hxsl can infer this)
- normal untagged variables are "pipeline variables" which can be either shader local vars, varying our output variables depending on the pipeline analysis (see below)

## Shader pipeline

Unlike other shader models (GLSL, HLSL, etc.), HXSL allows several shaders to be **linked ** together at runtime, allowing you to split your shader into several distinct effects that can be enabled/disabled easily, without having to maintain a single "Uber Shader" with all the possible combinations.

For instance, this is an shader that will affect all normals based on the previous shader:

```haxe
class MyEffect extends hxsl.Shader {
    static var SRC = {
        var transformedNormal : Vec3;
        function vertex() {
            transformedNormal.x *= -1.;
        }
    };
}
```

The effect can be added to each material **list of shaders**. 

HXSL will then compile this list into a single shader instance, based on a desired list of **outputs**. 

For instance, we can select either the `output.color` or `output.normal` declared in `MyShader` to either write the normal value or the color value (or we could select both if we want to write to several textures at once, like in [MRT](https://en.wikipedia.org/wiki/Multiple_Render_Targets))

After we know the list of the shaders and the desired output, HXSL will be linking the shaders together, ensuring that all variables read have been previously written (for instance `MyEffect` requires a `transformedNormal` variable that has been written by another shader in the pipeline).

Once linking is finished, HXSL will be optimizing out all the unused parts, such as:

* branches implying `@const` variables are optimized out when they are unreachable
* unused variables, unused textures, operations not performing any side effect that affects one of the outputs

The final resulting shader will then be compiled to the target platform native shader language (HXSL currently supports GLSL for OpenGL, HLSL for DirectX 11+, AGAL for AdobeAir, and PSSL for Sony PS4).

# Shader examples

There are several shader examples available in the [h3d.shader](api/h3d/shader) package.

These shaders mostly uses definitions present in either:

* [Base2d](api/h3d/shader/Base2d.hx) for all 2D display
* [BaseMesh](api/h3d/shader/BaseMesh.hx) for all 3D display
* [ScreenShader](api/h3d/shader/ScreenShader.hx) for fullscreen effect / post-process
