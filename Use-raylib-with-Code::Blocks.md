Guide provided by [Mark in raylib forum](http://forum.raylib.com/index.php?p=/discussion/133/raylib-in-code-blocks-a-how-to)

First, under Project Settings check the box that says, *"This is a custom Makefile"*.

Then, in your project's folder create a file named Makefile and paste the following:
```
files = main.c
output = main.exe

Debug:
	gcc -g -o obj\Debug\$(output) $(files) \
		c:\raylib\raylib\raylib_icon \
		-Ic:\raylib\raylib\src \
		-Lc:\raylib\MinGW\bin \
		-Lc:\raylib\MinGW\include\GLFW \
		-Iexternal -lraylib -lglfw3 -lopengl32 -lgdi32 -lopenal32 -lwinmm \
		-std=c99 -Wl,-allow-multiple-definition -Wl,--subsystem,windows -Wall

Release:
	gcc -s -o obj\Debug\$(output) $(files) \
		c:\raylib\raylib\raylib_icon \
		-Ic:\raylib\raylib\src \
		-Lc:\raylib\MinGW\bin \
		-Lc:\raylib\MinGW\include\GLFW \
		-Iexternal -lraylib -lglfw3 -lopengl32 -lgdi32 -lopenal32 -lwinmm \
		-std=c99 -Wl,-allow-multiple-definition -Wl,--subsystem,windows

cleanDebug:
	del /F /Q obj\Debug\*.*

cleanRelease:
	del /F /Q obj\Release\*.*
```
Change files and output variables as you see fit.