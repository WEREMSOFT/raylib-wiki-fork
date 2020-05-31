[example](#example) | [synopsis](#synopiys) | [defined](#defined)| [notes](#notes)
# WindowShouldClose 
Check if KEY_ESCAPE pressed or Close icon pressed

## Synopsis
```C
bool WindowShouldClose(void);  
```

**return value**: true if the call to ```InitWindow``` succeed.

## Defined
```raylib.h```

## Example:

```C
#include "raylib.h"

int main(void)
{

    InitWindow(screenWidth, screenHeight, "Hello Window!");
    SetTargetFPS(60);

    // Main game loop
    // Detect window close button or ESC key
    while (!WindowShouldClose())
    {
        
        BeginDrawing();

            ClearBackground(RAYWHITE);

            DrawText("Congrats! You created your first window!", 190, 200, 20, LIGHTGRAY);

        EndDrawing();
    }

    // Close window and OpenGL context
    CloseWindow();

    return 0;
}
```