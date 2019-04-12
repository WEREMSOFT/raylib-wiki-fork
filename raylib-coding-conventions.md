This page list some coding conventions rules I try to follow on raylib C code:

Code element | Convention | Example
--- | :---: | ---
Macros | ALL_CAPS | `#define MIN(a,b) (((a)<(b))?(a):(b))`
Defines | ALL_CAPS | `#define PLATFORM_DESKTOP`
Constants | ALL_CAPS | `const int NUMBER = 8`
Struct | TitleCase | `struct Texture2D`
Struct members |lowerCase | `texture.id`
Enum | TitleCase | `TextureFormat`
Enum members | ALL_CAPS | `UNCOMPRESSED_R8G8B8`
Functions | TitleCase or prefixTitleCase | `InitWindow()`
Variables | lowerCase | `screenWidth`
Local variables | lowerCase | `playerPosition`
Global variables | lowerCase | `fullscreen`
Operators | value1*value2 | `int product = value*6;`
Operators | value1/value2 | `int division = value/4;`
Operators | value1 + value2 | `int sum = value + 10;`
Operators | value1 - value2 | `int res = value - 5;`
Pointers | MyType *pointer | `Texture2D *array;`
float values | always x.xf | `float value = 10.0f`
Ternary Operator | (condition)? result1 : result2 | `printf("Value is 0: %s", (value == 0)? "yes" : "no");`

raylib code does not use TABS, use 4 spaces instead.

When dealing with braces or curly brackets, open-close them in aligned mode, it's more visual for students:
```c
void SomeFunction()
{
   // TODO: Do something here!
}
```
When proposing new functions, please try to use a clear naming for function-name and functions-parameters, in case of doubt, open ans issue for discussion.
