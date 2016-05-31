Dealing with custom shaders and make them generic is not an easy task. There are many things to consider for a shader because, after all, the shader is the responsible to process all the data (mesh, materials, textures, lighting) send to the GPU to generate the final frame.

Find an unified generic shader to deal with all kind of stuff is very complicated and, after analysing some of the big engines out there, I decided to go for a custom uber-shader-based solution.

By default, raylib shader struct support the following data:
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

On shader loading, following fixed location names are expected for maps:
```glsl
uniform sampler2D texture0;   // GL_TEXTURE0
uniform sampler2D texture1;   // GL_TEXTURE1
uniform sampler2D texture2;   // GL_TEXTURE2
```

Shaders are also directly related to Material struct:
```c
// Material type
typedef struct Material {
    Shader shader;              // Standard shader (supports 3 map textures)

    Texture2D texDiffuse;       // Diffuse texture  (binded to shader mapTexture0Loc)
    Texture2D texNormal;        // Normal texture   (binded to shader mapTexture1Loc)
    Texture2D texSpecular;      // Specular texture (binded to shader mapTexture2Loc)
    
    Color colDiffuse;           // Diffuse color
    Color colAmbient;           // Ambient color
    Color colSpecular;          // Specular color
    
    float glossiness;           // Glossiness level (Ranges from 0 to 1000)
} Material;
```
Where three texture maps (texDiffuse, texNormal, texSpecular) are binded to shader location points.

On drawing, depending on textures assigned to material and selected shader, they are internally binded on drawing or not:
```c
// Default material loading example
Material material = LoadDefaultMaterial();                 // Default shader assigned to material (supports  only diffuse map)
material.texDiffuse = LoadTexture("wood_diffuse.png");     // texture unit 0 activated (available in material shader)
material.texSpecular = LoadTexture("wood_specular.png");   // texture unit 2 activated (available in material shader)

// Standard material loading example
Material material = LoadStandardMaterial();                // Standard shader assigned to material (supports diffuse, normal and specular maps)
material.texDiffuse = LoadTexture("wood_diffuse.png");     // texture unit 0 activated (available in material shader)
material.texSpecular = LoadTexture("wood_specular.png");   // texture unit 2 NOT activated (not available in material shader)
```

Despite its name on material struct (`texDiffuse`, `texNormal`, `texSpecular`), user is free to use those maps in any way inside the **custom** shader.
