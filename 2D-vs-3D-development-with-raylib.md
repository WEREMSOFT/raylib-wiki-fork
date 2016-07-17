raylib was primary conceived for 2D game programming but with some basic 3D support. Every new raylib versión, 3D support has expanded to the point of including shaders and postprocessing effects. Dealing with 2D and 3D in an easy way supposed some technical trades that maybe were not the perfect solution for 2D neither 3D but work very well for both systems cohexistance.

####Three different buffers

raylib creates a bunch of vertex buffers (VBO) at initialization. Those buffers are used independently for 2D and 3D drawing and are updated in a frame-basis (dynamic buffers). Buffers size has been adjusted for a good performance. Depending on the information stored in each buffers-group, we have:

 - **LINES** (2 VBOs: [position, color]) - Store information for lines drawing, two vertex per line.
 - **TRIANGLES** (2 VBOs: [position, color]) - Store information for triangles, three vertex per triangle.
 - **QUADS** (4 VBOs:[position, texcoords, color, indices]) - Store information for quads, four vertex per quad + 6 index per quad.

Buffers are created at `InitWindow()`, vertex data is stored into buffers with every `Draw*()` function and buffers are processed for drawing on `EndDrawing()`. Internally, they are processed by function [`rlglDraw()`](https://github.com/raysan5/raylib/blob/master/src/rlgl.c#L1242).

Depending on the element drawn, vertex are stored in LINES, TRIANGLES or QUADS buffers. That could suppose a problem because, despite including z-depth information, buffers are processed in an specific order: 
`(1) LINES -> (2) TRIANGLES -> (3) QUADS`

If no `DEPTH_TEST` is enabled, QUADS-based shapes will be always drawn in front of TRIANGLES and LINES; if `DEPTH_TEST` is enabled, drawing is resolved ok thanks to the depth-buffer... until we use semi-transparent shapes.

####Alpha Blending

Due to the fact that the three buffer types are processed independently, LINES fragments are generated before TRIANGLE fragments that are also generated before QUADS fragments. For that reason, despite having `DEPTH_TEST` working ok, if we try to draw a semi-transparent TRIANGLES-based shape in front of a QUAD-based shape, QUAD-based shape fragments are not ready yet to be blended with TRIANGLES-based shape. That's a problem.

####2D and 3D mixing

In addition to the above problem, we have 2D vs 3D mixing. We use the same buffers for 2D drawing and 3D drawing; when changing to 3D mode (`Begin3dMode()`) buffers are processed and reseted to deal with 3D vertex data. Same way, when switching back to 2D mode (`End3dMode()`) buffers are processed and reseted to deal with 2D vertex data.

Sharing buffers for 2D and 3D is simple and works ok but presents a problem when mixing 2D and 3D: `DEPTH_TEST`. 2D screen elements are drawn in front of or behind the 3D elements; we don't want that.

An issue was introduced to try to find a solution for all this problems: [Issue 89](https://github.com/raysan5/raylib/issues/89)

A first idea was adding additional LINES and TRIANGLES buffers only for 3D but it was discarded due to the extra complexity and memory usage for raylib, considering that most of the time raylib users will probably use the library for 2D games and even in the case of using 3D, loaded 3D models are stored in separate static buffers. No need to overload the library with a new set of dinamic buffers.

After some thinking, one solutions was introduced, probably not the best one, but it gives a balance between 2D and 3D drawing, depth testing and blending.

**Solution:** Disable `DEPTH_TEST` for 2D and enable it only for 3D. The problem with this solution (a part of not having `DEPTH_TEST` on 2D) was the blending between elements from different buffers (i.e. TRIANGLES and QUADS). It was partially solved moving some commonly-used 2D elements (Circle, RectangleLines) to QUADS usage. That way, `DEPTH_TEST` and blending works ok with those elements. (Probably a final solution would be moving all the elements drawing to a single buffer type: QUADS).