raylib input system basically uses an event polling mechanism, centralized on raylib [core module](https://github.com/raysan5/raylib/blob/master/src/core.c).

At the end of the game loop, `EndDrawing()` function is called. This function calls `SwapBuffers()` and `PollInputEvents()`, multiple input devices are scanned at that moment and states are reqistered.

Depending on the platform, input events are managed using different mechanisms. 

Default supported input systems are: `KEYBOARD`, `MOUSE`, `GAMEPAD`, `TOUCH` 

PLATFORM_DESKTOP
---------------------
On this platform GLFW3 library ([rglfw](https://github.com/raysan5/raylib/blob/master/src/rglfw.c)) is used for input management. Events polling is done using `glfwPollEvents()`.

Input | Library | Details
:---: | :---: | ---
KEYBOARD | GLFW3 | Values are registered through callbacks `KeyCallback()`, `CharCallback()`
MOUSE | GLFW3 | Values are registered through callbacks `MouseButtonCallback()`, `MouseCursorPosCallback()`, `ScrollCallback()`
GAMEPAD | GLFW3 | Values registered by (no-callbacks): `glfwJoystickPresent()`, `glfwGetJoystickButtons()`, `glfwGetJoystickAxes()` *NOTE: Those functions do not require `glfwPollEvents()`*
TOUCH | - | Not available by default. Mouse clicks are mapped to touch events using gestures API.
 
PLATFORM_WEB
--------------
Some of the inputs in this platform are directly using GLFW3 because the library has been partially [ported to emscripten](https://github.com/emscripten-core/emscripten/blob/incoming/src/library_glfw.js). Despite that, gamepad and touch inputs are directly managed using emscripten API.

Input | Library | Details
:---: | :---: | ---
KEYBOARD | GLFW3 | Same as `PLATFOM_DESKTOP`, uses GLFW3 JS implementation
MOUSE | GLFW3 | Same as `PLATFOM_DESKTOP`, uses GLFW3 JS implementation
GAMEPAD | Emscripten | Using emscripten gamepad API: `EmscriptenGamepadCallback()`
TOUCH | Emscripten | Using emscripten touch API: `EmscriptenTouchCallback()`

PLATFORM_ANDROID
------------------
Android NDK provides its own events polling system: `ALooper_pollAll()`, called on `PollInputEvents()`.

Using `AndroidInputCallback()`, all inputs can be later processed. Currently only some inputs are processed but additional inputs could be processed

Input | Library | Details
:---: | :---: | ---
KEYBOARD | Android NDK | Keys registered on `AndroidInputCallback()`
MOUSE | - | Mouse inputs are actually mapped with TOUCH input
GAMEPAD | Android NDK | Support *partially* implemented on `AndroidInputCallback()`
TOUCH | Android NDK | Touch points registered on `AndroidInputCallback()`

PLATFORM_RPI
-------------
Probably this platform inputs polling is the most complex one. Some input events are polled by multiple secondary threads.

Keyboard polling is a special case, it can use directly `stdin` inputs to read keycodes. In that case, `stdin` is being reconfigured for ASCII chars reading and reseted at the end of the program... This approach presents several problems like a standard input lose if program is not closed properly but it does not require `X.org` libraries or superuser rights... Second option is being implemented to avoid `stdin` usage.

Input | Library | Details
:---: | :---: | ---
KEYBOARD | - | `stdin` is remapped to read keys using: `InitKeyboard()`, `ProcessKeyboard()`, `RestoreKeyboard()`
KEYBOARD | libevdev | Using standard `input_event` to read events from input device
MOUSE | libevdev | **Threaded**: `InitMouse()` -> `EventThreadSpawn()` -> `EventThread()`, uses `input_event`
GAMEPAD | joystick.h | **Threaded**: `InitGamepad()` -> `GamepadThread()`, uses `js_event`
TOUCH | libevdev | *Share `MOUSE` implementation*

Probably inputs could be further improved in all platforms, specially TOUCH inputs support and mapping with MOUSE.
