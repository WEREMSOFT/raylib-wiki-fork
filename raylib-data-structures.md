raylib provides some basic data structures to organize game information. Those structures are quite common in most of the engines out there:
```c
    // Basic data structures
    struct Color;           // Color type, RGBA (32bit)
    struct Rectangle;       // Rectangle type
    struct Vector2;         // Vector2 type
    struct Vector3;         // Vector3 type
    struct Matrix;          // Matrix type (OpenGL style 4x4)
    
    // 2D data (pixels, font...)
    struct Image;           // Image type (data stored in CPU memory (RAM))               
    struct Texture2D;       // Texture2D type (data stored in GPU memory (VRAM))
    struct RenderTexture2D; // RenderTexture2D type, for texture rendering
    struct SpriteFont;      // SpriteFont type, includes texture and chars data
    
    struct Camera;          // Camera type, defines 3d camera position/orientation
    struct Camera2D;        // Camera2D type, defines a 2d camera

    // 3D data (vertex, material properties...)
    struct Mesh;            // Vertex data definning a mesh
    struct Shader;          // Shader type (generic shader)
    struct Material;        // Material type
    struct Model;           // Basic 3d Model type

    struct Ray;             // Ray type (useful for raycast)
    
    // Audio related data
    struct Wave;            // Wave type, defines audio wave data
    struct Sound;           // Basic Sound source and buffer
    struct Music;           // Music type (file streaming from memory)
    struct AudioStream;     // Raw audio stream type
```