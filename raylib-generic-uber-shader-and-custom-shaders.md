Dealing with custom shaders and making them generic is not an easy task. There are many things to consider for a shader because, after all, the shader is responsible for processing all the data sent to the GPU (mesh, materials, textures, lighting) to generate the final frame.

Finding a unified generic shader to deal with all kinds of stuff is very complicated and, after analyzing some of the big engines out there, I decided to go for a custom uber-shader-based solution.

By default, raylib's shader struct is defined as:
```c
typedef struct Shader {
    unsigned int id;                // Shader program id
    int locs[MAX_SHADER_LOCATIONS]; // Shader locations array
} Shader;
```

This struct provides an array to store shader locations, those locations can be accessed by position using predefined values for convenience:
```c
// Shader location point type
typedef enum {
    LOC_VERTEX_POSITION = 0,
    LOC_VERTEX_TEXCOORD01,
    LOC_VERTEX_TEXCOORD02,
    LOC_VERTEX_NORMAL,
    LOC_VERTEX_TANGENT,
    LOC_VERTEX_COLOR,
    LOC_MATRIX_MVP,
    LOC_MATRIX_MODEL,
    LOC_MATRIX_VIEW,
    LOC_MATRIX_PROJECTION,
    LOC_VECTOR_VIEW,
    LOC_COLOR_DIFFUSE,
    LOC_COLOR_SPECULAR,
    LOC_COLOR_AMBIENT,
    LOC_MAP_ALBEDO,          // LOC_MAP_DIFFUSE
    LOC_MAP_METALNESS,       // LOC_MAP_SPECULAR
    LOC_MAP_NORMAL,
    LOC_MAP_ROUGHNESS,
    LOC_MAP_OCCUSION,
    LOC_MAP_EMISSION,
    LOC_MAP_HEIGHT,
    LOC_MAP_CUBEMAP,
    LOC_MAP_IRRADIANCE,
    LOC_MAP_PREFILTER,
    LOC_MAP_BRDF
} ShaderLocationIndex;

#define LOC_MAP_DIFFUSE      LOC_MAP_ALBEDO
#define LOC_MAP_SPECULAR     LOC_MAP_METALNESS
```

On shader loading, the following location names are checked:
```glsl
uniform mat4 mvp;             // VS: ModelViewProjection matrix
uniform mat4 projection;      // VS: Projection matrix
uniform mat4 view;            // VS: View matrix
uniform vec4 colDiffuse;      // FS: Diffuse color
uniform sampler2D texture0;   // FS: GL_TEXTURE0
uniform sampler2D texture1;   // FS: GL_TEXTURE1
uniform sampler2D texture2;   // FS: GL_TEXTURE2
```

Shaders are also directly related to the Material struct:
```c
// Material type
typedef struct Material {
// Material type (generic)
typedef struct Material {
    Shader shader;          // Material shader
    MaterialMap maps[MAX_MATERIAL_MAPS]; // Material maps
    float *params;          // Material generic parameters (if required)
} Material;
```

Material support by default a number of maps (texture and properties) that can be accessed for convenience using the provided values:

```c
// Material map type
typedef enum {
    MAP_ALBEDO    = 0,       // MAP_DIFFUSE
    MAP_METALNESS = 1,       // MAP_SPECULAR
    MAP_NORMAL    = 2,
    MAP_ROUGHNESS = 3,
    MAP_OCCLUSION,
    MAP_EMISSION,
    MAP_HEIGHT,
    MAP_CUBEMAP,             // NOTE: Uses GL_TEXTURE_CUBE_MAP
    MAP_IRRADIANCE,          // NOTE: Uses GL_TEXTURE_CUBE_MAP
    MAP_PREFILTER,           // NOTE: Uses GL_TEXTURE_CUBE_MAP
    MAP_BRDF
} TexmapIndex;

#define MAP_DIFFUSE      MAP_ALBEDO
#define MAP_SPECULAR     MAP_METALNESS
``` 

When drawing, maps are internally bound or not depending on the availability:
```c
// Default material loading example
Material material = LoadMaterialDefault();                      // Default shader assigned to material
material.maps[MAP_DIFFUSE] = LoadTexture("tex_diffuse.png");    // texture unit 0 activated (available in material shader)
material.maps[MAP_SPECULAR] = LoadTexture("tex_specular.png");  // texture unit 1 activated (available in material shader)
```

User can load any custom shader using provided `Material` and `Shader`structs:

```c
Material material = { 0 };     // Empty material

material.shader = LoadShader("custom_shader.vs", "custom_shader.fs);

// Setup location points in case names are not predefined ones or more locations are required
// Use: GetShaderLocation() and SetShaderValue*() functions

material.maps[0] = LoadTexture("tex_albedo.png");
material.maps[1] = LoadTexture("tex_metalness.png");
material.maps[2] = LoadTexture("tex_normal.png");
```
