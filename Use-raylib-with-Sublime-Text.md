**[_Sublime Text 3_](https://www.sublimetext.com/)**, fast and simple code editor.

**Note:** The following how-to is mostly for Windows. If you know a way to do it on Mac and/or Linux, feel free to edit this wiki and add it. 

### Step 1
Install the latest release of Raylib from the **[releases page](https://github.com/raysan5/raylib/releases/tag/2.5.0)**. If you're on Windows installing Raylib with MingW (GCC) or TCC included will make it easier.

### Step 2
Depending on how easy you want to make it for yourself, create a file called `build.bat` in the root of your current project (recommended) or in `%AppData%\Sublime Text 3\Packages\User`

Make the `build.bat` script look something like this:  
```bat
@echo off
cls

:: ***********************************************************************
   rem Batch file for compiling Raylib and/or Raygui applications
:: ***********************************************************************

:Initialization
	echo ^> Initializing Variables
	echo ----------------------------
	SET "RAYLIB_DIR=C:\raylib"
	SET "CURRENT_DIR=%cd%"

	SET "INPUT_FILE=%1"
	SET "OUTPUT_FILE=%2"
	SET "INCLUDE_FILES="

	SET "COMPILER=%RAYLIB_DIR%\tcc\tcc.exe"
	SET "CFLAGS=%RAYLIB_DIR%\raylib\src\raylib.rc.data -std=c99 -Wall"
	SET "LDFLAGS=-lmsvcrt -lraylib -lopengl32 -lgdi32 -lwinmm -lkernel32 -lshell32 -luser32 -Wl,-subsystem=gui"
	SET "EXTRAFLAGS="

	IF /I "%3"=="Release" SET EXTRAFLAGS=%EXTRAFLAGS% -b

:Main
	echo(
	echo ^> Removing Previous Build
	echo ----------------------------
	IF EXIST "%1.exe" del /F "%1.exe"

	echo(
	echo ^> Compiling Program
	echo ----------------------------
	%COMPILER% -o "%OUTPUT_FILE%" "%INPUT_FILE%" %INCLUDE_FILES% %CFLAGS% %LDFLAGS% %EXTRAFLAGS%
```

Make sure to replace the variables in `:Initialization` with the proper variables.

`RAYLIB_DIR` the path to the root of the Raylib directory  
`INPUT_FILE` the file the build was called from  (`%1` = `build.bat <arg1>`)  
`OUTPUT_FILE` the output file the build should compile to (`%2` = `build.bat <arg1> <arg2>`)  
`INCLUDE_FILES` files to include when compiling (this is why it's recommended to create `build.bat` in your project directory, this way you won't have to navigate to Sublime Text user packages each time you want to change something)  
`COMPILER` the complete path to the compiler you want to use  
`EXTRAFLAGS` extra compiler flags (if any)  

### Step 3
In Sublime Text, go to `Tools > Build System > New Build System...`.

Make your sublime-build look something like this:  
```json
{
	"selector" : "source.c",

	"quiet": true,

	"shell" : true,

	"working_dir" : "$file_path",

	"variants": [
		{
		"name": "Compile",
		"cmd" : ["$file_path\\build.bat", "$file_name", "$file_base_name.exe"]
		},

		{
		"name": "Compile & Run",
		"cmd" : ["$file_path\\build.bat", "$file_name", "$file_base_name.exe", "&&", "$file_base_name.exe"], //, "&&", "$file_base_name.exe"
		},

		{
		"name": "Release",
		"cmd" : ["$file_path\\build.bat", "$file_name", "$file_base_name.exe", "Release"]
		}
	]
}
```

**Note:** If you decided to place `build.bat` in `%AppData%\Sublime Text 3\Packages\User`, you need to change `"$file_path\\build.bat"` to `"$packages\\user\\build.bat"`

### Step 4
Open your Raylib project in Sublime Text > Tools > Build System (or **Ctrl** + **Shift** + **B**) > Select the Raylib build system that you want to use.

_or_

You can build the game using **View > Command Palette** (or **Ctrl** + **Shift** + **P**) > Type **Run Task** > Press **Enter** > Select the Raylib build system that you want to use.


A Windows executable is created in your project folder.
