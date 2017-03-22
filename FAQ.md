## **Can I run raylib in my old PC?**

Yes, you can. raylib comes compiled by default for OpenGL 3.3 backend but it can be recompiled for older OpenGL versions! It can be recompiled for OpenGL 1.1 (1997) and OpenGL 2.1 (2006). To do that using Notepad++, just follow this steps:

1. Open file `C:\raylib\raylib\src\core.c`
2. Execute Notepad++ script: `raylib_source_compile_gl11` or `raylib_source_compile_gl21`, those scripts recompile raylib to desired OpenGL version and copy all required data to the required folders
3. Open any raylib example or your raylib game
4. Execute Notepad++ script: `raylib_compile_execute`

Note that above process only needs to be done once, if at some point you need to change OpenGL version again, just repeat that process.