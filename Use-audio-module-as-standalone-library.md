raylib audio module can be used as standalone library

```c
/*******************************************************************************************
*
*   raylib audio module as standalone library sample
*
*   NOTE: Sample dependencies:
*       stb_vorbis - To load OGG data 
*
*   raylib audio module is distributed with raylib 1.3 (www.raylib.com)
*   raylib is licensed under an unmodified zlib/libpng license (View raylib.h for details)
*
*   Copyright (c) 2015 Ramon Santamaria (@raysan5)
*
********************************************************************************************/

#include <stdio.h>
#include <conio.h>      // Windows only, no stardard library

#include "audio.h"

#define KEY_ESCAPE  27

int main()
{
    // Initialization
    //--------------------------------------------------------------------------------------
    unsigned char key;
    
    InitAudioDevice();
    
    Sound fxWav = LoadSound("resources/weird.wav");        // Load WAV audio file
    Sound fxOgg = LoadSound("resources/tanana.ogg");      // Load OGG audio file
    
    PlayMusicStream("resources/ambient.ogg");

    printf("\nPress s or d to play sounds...\n");
    //--------------------------------------------------------------------------------------
    
    while (key != KEY_ESCAPE)
    {
        // Update
        //----------------------------------------------------------------------------------
        if (kbhit()) key = getch();

        if (key == 's') PlaySound(fxWav);
        if (key == 'd') PlaySound(fxOgg);
        
        key = 0;
        
        UpdateMusicStream();
        //--------------------------------------------------------------------------------------
        
        // Draw
        //----------------------------------------------------------------------------------
        // TODO: Something to draw?
        //----------------------------------------------------------------------------------
    }
    
    // De-Initialization
    //--------------------------------------------------------------------------------------
    UnloadSound(fxWav);     // Unload sound data
    UnloadSound(fxOgg);     // Unload sound data
    
    CloseAudioDevice();
    //--------------------------------------------------------------------------------------
    
    printf("\n\nPress ENTER to close...");
    getchar();

    return 0;
}
```