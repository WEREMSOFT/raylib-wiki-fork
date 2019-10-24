raylib provides some basic data structures to organize game information. Those structures are quite common in most of the engines out there:
```c
    // Basic data structures
    struct Color;           // Color type, RGBA (32bit)
    struct Rectangle;       // Rectangle type
    struct Vector2;         // Vector2 type
    struct Vector3;         // Vector3 type
    struct Vector4;         // Vector4 type
    struct Matrix;          // Matrix type (OpenGL style 4x4)
    struct Quaternion;      // Alias for Vector4

    
    // 2D data (pixels, font...)
    struct Image;           // Image type (data stored in CPU memory (RAM))
    struct Texture;         // Alias for Texture2D               
    struct Texture2D;       // Texture2D type (data stored in GPU memory (VRAM))
    struct RenderTexture    // Alias for RenderTexture
    struct RenderTexture2D; // RenderTexture2D type, for texture rendering
    struct CharInfo;        // Font Character Info
    struct Font;            // Font type, includes texture and charSet array data (SpriteFont fallback)
    
    struct Camera;          // Alias for Camera3D
    struct Camera2D;        // Camera2D type, defines a 2d camera
    struct Camera3D;        // Camera type, defines a camera position/orientation in 3d space

    // 3D data (vertex, material properties...)
    struct Mesh;            // Vertex data defining a mesh, animation vertex data
    struct Shader;          // Shader type (generic shader)
    struct Material;        // Material type
    struct MaterialMap      // MaterialMap type
    struct Model;           // Basic 3d Model type
    struct ModelAnimation;  // Skeletal and frame information
    struct BoneInfo;        // Bone information

    struct Transform;       // Transform properties, translation, rotation, scale..

    struct Ray;             // Ray type (useful for raycast)
    struct RayHitInfo;      // Information related to hit from Ray
    struct BoundingBox;     // Simple Bounding Box
    
    // Audio related data
    struct Wave;            // Wave type, defines audio wave data
    struct Sound;           // Basic Sound source and buffer
    struct Music;           // Music type (file streaming from memory)
    struct AudioStream;     // Raw audio stream type
```