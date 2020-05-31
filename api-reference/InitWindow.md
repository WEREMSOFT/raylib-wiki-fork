[example](#example) | [synopsis](#synopiys) | [defined](#defined) | [notes](#notes)
# InitWindow
Initialize window and OpenGL context.

### Notes
Call this first. If you try to call other function that make calls to the drawing api, your program will crash. 

As an example, you can not call ```LoadTexture```, since it interacts with the GPU memory.

## Synopsis
```
void InitWindow(int width, int height, const char title);
```

**int width:** The width of the window in pixels.
**int height:** The height of the window in pixels.
**const char \*title:** Its intended to be a string literal. the title of the window. Depends on the OS implementation.

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