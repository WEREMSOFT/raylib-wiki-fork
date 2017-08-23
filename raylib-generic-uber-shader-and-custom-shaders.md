Dealing with custom shaders and making them generic is not an easy task. There are many things to consider for a shader because, after all, the shader is responsible for processing all the data sent to the GPU (e.g. mesh, materials, textures, lighting) to generate the final frame.

Finding a unified generic shader to deal with all kinds of stuff is very complicated and, after analyzing some of the big engines out there, I decided to go for a custom uber-shader-based solution.

By default, raylib's shader struct supports the following data:
```c
typedef struct Shader {
    unsigned int id;      // Shader program id
    
    // Vertex attributes locations (default locations)
    int vertexLoc;        // Vertex attribute location point    (default-location = 0)
    int texcoordLoc;      // Texcoord attribute location point  (default-location = 1)
    int texcoord2Loc;     // Texcoord2 attribute location point (default-location = 5)
    int normalLoc;        // Normal attribute location point    (default-location = 2)
    int tangentLoc;       // Tangent attribute location point   (default-location = 4)
    int colorLoc;         // Color attibute location point      (default-location = 3)

    // Uniform locations
    int mvpLoc;           // ModelView-Projection matrix uniform location point (vertex shader)
    int tintColorLoc;     // Diffuse color uniform location point (fragment shader)
    
    // Texture map locations (generic for any kind of map)
    int mapTexture0Loc;  // Map texture uniform location point (default-texture-unit = 0)
    int mapTexture1Loc;  // Map texture uniform location point (default-texture-unit = 1)
    int mapTexture2Loc;  // Map texture uniform location point (default-texture-unit = 2)
} Shader;
```

As you can see, most of the location points are pre-defined **on shader loading**; custom shaders developed for raylib must follow those conventions.

On shader load, the following fixed location names are expected for maps:
```glsl
uniform sampler2D texture0;   // GL_TEXTURE0
uniform sampler2D texture1;   // GL_TEXTURE1
uniform sampler2D texture2;   // GL_TEXTURE2
```

Shaders are also directly related to the Material struct:
```c
// Material type
typedef struct Material {
    Shader shader;              // Standard shader (supports 3 map textures)

    Texture2D texDiffuse;       // Diffuse texture  (bound to shader mapTexture0Loc)
    Texture2D texNormal;        // Normal texture   (bound to shader mapTexture1Loc)
    Texture2D texSpecular;      // Specular texture (bound to shader mapTexture2Loc)
    
    Color colDiffuse;           // Diffuse color
    Color colAmbient;           // Ambient color
    Color colSpecular;          // Specular color
    
    float glossiness;           // Glossiness level (Ranges from 0 to 1000)
} Material;
```
Where three texture maps (texDiffuse, texNormal, texSpecular) will bind to shader location points.

When drawing, textures are internally bound or not depending on the selected shader and material:
```c
// Default material loading example
Material material = LoadDefaultMaterial();                 // Default shader assigned to material (supports only diffuse map)
material.texDiffuse = LoadTexture("wood_diffuse.png");     // texture unit 0 activated (available in material shader)
material.texSpecular = LoadTexture("wood_specular.png");   // texture unit 2 activated (available in material shader)

// Standard material loading example
Material material = LoadStandardMaterial();                // Standard shader assigned to material (supports diffuse, normal and specular maps)
material.texDiffuse = LoadTexture("wood_diffuse.png");     // texture unit 0 activated (available in material shader)
material.texSpecular = LoadTexture("wood_specular.png");   // texture unit 2 NOT activated (not available in material shader)
```

Despite its name on material struct (`texDiffuse`, `texNormal`, `texSpecular`), the user is free to use those maps in any way inside the **custom** shader.
