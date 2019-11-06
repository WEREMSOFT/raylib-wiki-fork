raylib provides some basic data structures to organize game data. 
Those structures are quite common in most of the engines out there.

### raylib data structures
```c
    // Basic data structures
    struct Color;            [ 4 bytes]    // RGBA values, 4 char, 32bit color
    struct Rectangle;        [16 bytes]    // 4 float values
    struct Vector2;          [ 8 bytes]    // 2 float values
    struct Vector3;          [12 bytes]    // 3 float values
    struct Vector4;          [16 bytes]    // 4 float values
    struct Matrix;           [64 bytes]    // 16 float values, right handed, column major
    struct Quaternion;       [16 bytes]    // Vector4 alias

    // 2D data structures (pixels, font...)
    struct Image;            [20 bytes]    // Image data pointer (RAM) and 4 data parameters
    struct Texture2D;        [20 bytes]    // OpenGL texture id (VRAM) and basic info
    struct Texture;          [20 bytes]    // Texture2D alias
    struct TextureCubemap;   [20 bytes]    // OpenGL cubemap texture id and basic info
    struct RenderTexture2D;  [28 bytes]    // OpenGL framebuffer id and color+depth textures
    struct RenderTexture     [28 bytes]    // RenderTexture2D alias

    struct NPatchInfo        [36 bytes]    // Source rectangle and border offsets
    struct CharInfo;         [32 bytes]    // One character image and info properties
    struct Font;             [36 bytes]    // Texture atlas and recs+chars data array pointers
    
    // Screen view structures
    struct Camera2D;         [24 bytes]    // 2D camera offset, target, rotation and zoom
    struct Camera3D;         [44 bytes]    // 3D camera position, target, up vectors and parameters
    struct Camera;           [44 bytes]    // Camera3D alias
    struct VrDeviceInfo;     [64 bytes]    // Head-Mounted-Display device configuration parameters
   
    // 3D data structures (vertex, material properties...)
    // NOTE: Those structures are more complex so they use some internal pointers to data
    struct Mesh;             [60 bytes]    // Vertex data, OpenGL buffers ids, animation data (skeleton bones and pose)
    struct Shader;           [ 8 bytes]    // OpenGL program id, locations array pointer
    struct Material;         [16 bytes]    // Shader and maps array pointer
    struct MaterialMap       [28 bytes]    // Texture, color and value
    struct Model;            [96 bytes]    // Meshes+materials array pointers, transform matrix (64 bytes)
    struct ModelAnimation;   [16 bytes]    // Skeletal bones data and frames transformation
    struct BoneInfo;         [36 bytes]    // Bone name (32 bytes) and parent id
    struct Transform;        [40 bytes]    // Vertex transformation: translation, rotation, scale

    struct Ray;              [24 bytes]    // Ray-casting position+direction vectors
    struct RayHitInfo;       [32 bytes]    // Ray collision information
    struct BoundingBox;      [12 bytes]    // Defined by min and max vertex
 
    // Audio related data
    struct Wave;             [20 bytes]    // Wave data pointer (RAM) and data parameters
    struct AudioStream;      [16 bytes]    // Audio buffer pointer (private) and parameters
    struct Sound;            [20 bytes]    // Audio stream and samples count
    struct Music;            [32 bytes]    // Audio stream and music data pointer for streaming
```

raylib abuses the data pass-by-value on most of its functions, actually, only around 10% of the functions require dealing with data pointers. For this reason, I tried to keep data structures as small as possible, usually under 64 bytes size, and use internal pointers when data requires modification by some function (usually Load/Update/Unload functions).

### raylib functions that require passing data by reference to be modified inside the function
```c
// core.c
char **GetDirectoryFiles(const char *dirPath, int *count);  // Get filenames in a directory path (memory should be freed)
char **GetDroppedFiles(int *count);                         // Get dropped files names (memory should be freed)

// camera.h
void UpdateCamera(Camera *camera);                          // Update camera position for selected mode

// textures.c
// NOTE: By design, MOST of the [Image*()] functions require passing an [Image *image] for modification
void GenTextureMipmaps(Texture2D *texture);                 // Generate GPU mipmaps for a texture

// text.c
Image GenImageFontAtlas(const CharInfo *chars, Rectangle **recs, int charsCount, int fontSize, int padding, int packMethod);  // Generate image font atlas using chars info
const char **TextSplit(const char *text, char delimiter, int *count);         // Split text into multiple strings
int *GetCodepoints(const char *text, int *count);                             // Get all codepoints in a string, codepoints count returned by parameters
int GetNextCodepoint(const char *text, int *bytesProcessed);                  // Returns next codepoint in a UTF8 encoded string; 0x3f('?') is returned on failure
const char *CodepointToUtf8(int codepoint, int *byteLength);                  // Encode codepoint into utf8 text (char array length returned as parameter)

// models.c
Mesh *LoadMeshes(const char *fileName, int *meshCount);                       // Load meshes from model file
void SetMaterialTexture(Material *material, int mapType, Texture2D texture);  // Set texture for a material map type (MAP_DIFFUSE, MAP_SPECULAR...)
void SetModelMeshMaterial(Model *model, int meshId, int materialId);          // Set material for a mesh
ModelAnimation *LoadModelAnimations(const char *fileName, int *animsCount);   // Load model animations from file
void MeshTangents(Mesh *mesh);                                                // Compute mesh tangents
void MeshBinormals(Mesh *mesh);                                               // Compute mesh binormals
bool CheckCollisionRaySphereEx(Ray ray, Vector3 center, float radius, Vector3 *collisionPoint); // Detect collision between ray and sphere, returns collision point
void UpdateVrTracking(Camera *camera);                                        // Update VR tracking (position and orientation) and camera

// audio.c
void WaveFormat(Wave *wave, int sampleRate, int sampleSize, int channels);    // Convert wave data to desired format
void WaveCrop(Wave *wave, int initSample, int finalSample);                   // Crop a wave to defined samples range
```