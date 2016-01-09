rlgl is a raylib module that let the user program in an OpenGL immediate mode style on any platform (internally uses OpenGL 1.1, 3.3 or ES 2.0). It can be used as standalone library.

// TODO: Explain it a bit more

```c
/*******************************************************************************************
*
*   rlgl standalone sample
*
*   NOTE: Sample dependencies:
*       GLFW3 - Window/Context creation and input management 
*       GLEW - OpenGL 3.3 extensions loading
*       raymath - Required by rlgl for vectorial/matrix maths
*
*   rlgl module is distributed with raylib 1.3 (www.raylib.com)
*   raylib is licensed under an unmodified zlib/libpng license (View raylib.h for details)
*
*   Copyright (c) 2015 Ramon Santamaria (@raysan5)
*
********************************************************************************************/

#include <GLFW/glfw3.h>     // Required for windows/context management

#define RLGL_STANDALONE     // Use rlgl as standalone library (not raylib.h dependant)
#include "../src/rlgl.h"

// Vector2 data type
typedef struct {
    float x;
    float y;
} Vector2;


static void KeyCallback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
    if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
    {
        glfwSetWindowShouldClose(window, GL_TRUE);
    }
}

void DrawRectangleV(Vector2 position, Vector2 size, Color color);

int main(void)
{
    // Initialization
    //--------------------------------------------------------------------------------------
    const int screenWidth = 800;
    const int screenHeight = 450;
    
    GLFWwindow *window;
    
    if (!glfwInit()) exit(EXIT_FAILURE);    // Initialize GLFW3
   
    window = glfwCreateWindow(screenWidth, screenHeight, "rlgl standalone", NULL, NULL);
    
    if (!window)
    {
        glfwTerminate();
        exit(EXIT_FAILURE);
    }
    
    glfwMakeContextCurrent(window);
    glfwSwapInterval(1);
    glfwSetKeyCallback(window, KeyCallback);
    
    rlglInit();     // Initialize rlgl system (buffers)
    rlglInitGraphics(0, 0, screenWidth, screenHeight);
    rlClearColor(245, 245, 245, 255); // Define clear color
    
    // Box variables
    Vector2 boxSize = { 400, 200 };
    Vector2 boxPosition = { 100, 100 };
    Color boxColor = { 255, 0, 0, 255 };
    //--------------------------------------------------------------------------------------
    
    while (!glfwWindowShouldClose(window))
    {
        // Update
        //----------------------------------------------------------------------------------
        glfwPollEvents();       // Capture input events
        //----------------------------------------------------------------------------------

        // Draw
        //----------------------------------------------------------------------------------
        rlClearScreenBuffers();                 // Clear backbuffer
        
        DrawRectangleV(position, size, color);  // Load vertex buffers
        
        rlglDraw();                             // Draw everything
        
        glfwSwapBuffers(window);                // Copy backbuffer to frontbuffer
        //----------------------------------------------------------------------------------
    }
    
    // De-Initialization
    //--------------------------------------------------------------------------------------
    rlglClose();
    
    glfwDestroyWindow(window);
    glfwTerminate();
    //--------------------------------------------------------------------------------------
    
    return 0;
}

// Draw a rectangle (OpenGL immediate mode style)
void DrawRectangleV(Vector2 position, Vector2 size, Color color)
{
    rlBegin(RL_TRIANGLES);
        rlColor4ub(color.r, color.g, color.b, color.a);

        rlVertex2i(position.x, position.y);
        rlVertex2i(position.x, position.y + size.y);
        rlVertex2i(position.x + size.x, position.y + size.y);

        rlVertex2i(position.x, position.y);
        rlVertex2i(position.x + size.x, position.y + size.y);
        rlVertex2i(position.x + size.x, position.y);
    rlEnd();
}
```