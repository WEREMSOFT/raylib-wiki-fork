raylib uses [GLFW library](https://github.com/glfw/glfw) for several platforms Window/Input events management:

  - `PLATFORM_DESKTOP` (Windows, Linux, macOS)
  - `PLATFORM_WEB` (Html5) ([Emscripten JS implementation (limited)](https://github.com/emscripten-core/emscripten/blob/master/src/library_glfw.js))

GLFW is used only on [core](https://github.com/raysan5/raylib/blob/master/src/core.c) module. 

In case someone could be interested in replacing GLFW for a custom platform-specific implementation, here it is a list with the functions currently used (raylib 3.0):

```c
 // GLFW: Device init/close
glfwInit();
glfwInitHint(GLFW_COCOA_CHDIR_RESOURCES, GLFW_FALSE);
glfwDefaultWindowHints();
glfwWindowHint(GLFW_SCALE_TO_MONITOR, GLFW_TRUE);
glfwCreateWindow(CORE.Window.display.width, CORE.Window.display.height, CORE.Window.title, glfwGetPrimaryMonitor(), NULL);
glfwDestroyWindow(CORE.Window.handle);
glfwWindowShouldClose(CORE.Window.handle);
glfwSetWindowShouldClose(CORE.Window.handle, GLFW_TRUE);
glfwMakeContextCurrent(CORE.Window.handle);
glfwGetFramebufferSize(CORE.Window.handle, &fbWidth, &fbHeight);
glfwWaitEvents();
glfwPollEvents();
glfwSwapInterval(1);
glfwSwapBuffers(CORE.Window.handle);
glfwTerminate();

// GLFW: Window/Monitor management
glfwGetWindowPos(CORE.Window.handle, &CORE.Window.position.x, &CORE.Window.position.y);
glfwGetWindowAttrib(CORE.Window.handle, GLFW_VISIBLE) == GL_FALSE);
glfwSetWindowTitle(CORE.Window.handle, title);
glfwSetWindowPos(CORE.Window.handle, x, y);
glfwGetPrimaryMonitor();
glfwGetMonitors(&monitorCount);
glfwGetMonitorName(monitors[monitor]));
glfwGetMonitorPhysicalSize(monitors[monitor], &physicalWidth, NULL);
glfwSetWindowMonitor(CORE.Window.handle, monitors[monitor], 0, 0, mode->width, mode->height, mode->refreshRate);
glfwSetWindowSizeLimits(CORE.Window.handle, width, height, mode->width, mode->height);
glfwSetWindowSize(CORE.Window.handle, width, height);
glfwGetVideoMode(monitor);
glfwShowWindow(CORE.Window.handle);
glfwHideWindow(CORE.Window.handle);
glfwGetWin32Window(CORE.Window.handle);
glfwGetX11Window(window);
glfwGetCocoaWindow(window);

// GLFW: Misc functionality
glfwGetProcAddress();
glfwGetClipboardString(CORE.Window.handle);
glfwSetClipboardString(CORE.Window.handle, text);
glfwGetTime();

// GLFW: Callbacks (Window/Input events)
glfwSetErrorCallback(ErrorCallback);
glfwSetWindowSizeCallback(CORE.Window.handle, WindowSizeCallback);
glfwSetWindowIconifyCallback(CORE.Window.handle, WindowIconifyCallback);
glfwSetWindowFocusCallback(CORE.Window.handle, WindowFocusCallback);
glfwSetCursorEnterCallback(CORE.Window.handle, CursorEnterCallback);
glfwSetCursorPosCallback(CORE.Window.handle, MouseCursorPosCallback);
glfwSetMouseButtonCallback(CORE.Window.handle, MouseButtonCallback);
glfwSetScrollCallback(CORE.Window.handle, ScrollCallback);
glfwSetKeyCallback(CORE.Window.handle, KeyCallback);
glfwSetCharCallback(CORE.Window.handle, CharCallback);
glfwSetDropCallback(CORE.Window.handle, WindowDropCallback);

// GLFW: Input management
// NOTE: Most inputs (keyboard/mouse) are managed through callbacks
glfwJoystickPresent(i);
glfwGetJoystickName(gamepad);
glfwGetGamepadState(i, &state);
glfwSetInputMode(CORE.Window.handle, GLFW_CURSOR, GLFW_CURSOR_NORMAL);
glfwSetCursorPos(CORE.Window.handle, CORE.Input.Mouse.position.x, CORE.Input.Mouse.position.y);
```

GLFW is NOT used on the following platforms, where custom implementations are used to manage Window/Inputs:

  - `PLATFORM_ANDROID`-> Uses `native_app_glue` Android NDK module
  - `PLATFORM_RPI`(native, no desktop) -> Uses `EGL`, `evdev` and standard system libraries directly
  - `PLATFORM_UWP`-> Uses UWP provided libraries
