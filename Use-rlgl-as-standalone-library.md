rlgl is a raylib module that let the user program in an OpenGL immediate mode style on any platform (internally uses OpenGL 1.1, 3.3 or ES 2.0). It can be used as standalone library.

// TODO: Explain it a bit more

```c
#include <GLFW/glfw3.h>     // Manage window creation and context

#define RLGL_STANDALONE
#include "../src/rlgl.h"

#include <stdlib.h>
#include <stdio.h>

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

static void DrawRectangleV(Vector2 position, Vector2 size, Color color);

int main(void)
{
    const int screenWidth = 800;
    const int screenHeight = 450;
    
    GLFWwindow *window;
    
    glfwSetErrorCallback(ErrorCallback);
    
    if (!glfwInit()) exit(EXIT_FAILURE);
   
    window = glfwCreateWindow(screenWidth, screenHeight, "Testing rlgl standalone", NULL, NULL);
    
    if (!window)
    {
        glfwTerminate();
        exit(EXIT_FAILURE);
    }
    
    glfwMakeContextCurrent(window);
    glfwSwapInterval(1);
    glfwSetKeyCallback(window, KeyCallback);
    
    rlglInit();
    rlglInitGraphics(0, 0, screenWidth, screenHeight);
    rlClearColor(245, 245, 245, 255); // Define clear color
    
    Vector2 size = { 400, 200 };
    Vector2 position = { screenWidth/2 - size.x, screenHeight/2 - size.y};
    Color color = { 255, 0, 0, 255 };        // RED
    
    while (!glfwWindowShouldClose(window))
    {
        rlClearScreenBuffers();
        
        DrawRectangleV(position, size, color);
        
        rlglDraw();
        
        glfwSwapBuffers(window);
        glfwPollEvents();
    }
    
    rlglClose();
    
    glfwDestroyWindow(window);
    glfwTerminate();
    
    return 0;
}

static void DrawRectangleV(Vector2 position, Vector2 size, Color color)
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