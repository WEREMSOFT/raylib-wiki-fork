raylib was primary conceived for 2D game programming, but with some basic 3D support. Every new raylib version, 3D support has been improved to the point of including shaders and postprocessing effects. Dealing with 2D and 3D in an easy way required some technical trade-offs that may not be a perfect solutions for 2D or 3D, but work very well for both systems coexistence.

### Three separate internal buffers

raylib creates a bunch of vertex buffers (VBO) at initialization. Those buffers are used independently for 2D and 3D drawing and are updated in a frame-basis (_dynamic buffers_). The size of the buffers have been adjusted for a good performance. Depending on the information stored in each buffers-group, we have:

Usage | # VBOs | VBOs data | Description
--- | :---: | :---: | ---
LINES_BATCH | 2 | position<br> color | Stores information for lines drawing, two vertex per line.
TRIANGLES_BATCH | 2 | position<br> color | Stores information for triangles drawing, three vertex per triangle.
QUADS_BATCH | 4 | position<br> texcoords<br>color<br> indices | Stores information for quads drawing, four vertex per quad + 6 index per quad.

Buffers are created at `InitWindow()`, vertex data is stored into buffers with every `Draw*()` function and buffers are processed for drawing on `EndDrawing()`. Internally, they are processed by function [`rlglDraw()`](https://github.com/raysan5/raylib/blob/master/src/rlgl.c#L1249).

Depending on the element being drawn, vertices are stored in either `LINES`, `TRIANGLES` or `QUADS` buffers. That could suppose a problem because despite including z-depth information, buffers are processed in a very specific order: 
`(1) LINES -> (2) TRIANGLES -> (3) QUADS`

If no `DEPTH_TEST` is enabled, QUADS-based shapes will be always drawn in front of TRIANGLES and LINES; if `DEPTH_TEST` is enabled, drawing is resolved as expected thanks to the depth-buffer... until we use semi-transparent shapes.

### ISSUE: Alpha Blending

Due to the fact that the three buffer types are processed independently, `LINE` fragments are generated before `TRIANGLE` fragments, and both are generated before `QUAD` fragments. For that reason, despite having `DEPTH_TEST` working OK, a semi-transparent `TRIANGLES` based shape in front of a `QUADS` based shape cannot be blended. Similarly, a semi-transparent `LINES` based shape in front of a `TRIANGLES` or `QUADS` based shape cannot be blended. That's a problem.

### ISSUE: 2D and 3D mixing

In addition to the above problem, we have 2D vs 3D mixing. We use the same buffers for 2D drawing and 3D drawing. When changing to 3D mode (`Begin3dMode()`), buffers are processed and reseted to deal with 3D vertex data. Similarly, when switching back to 2D mode (`End3dMode()`), buffers are processed and reseted to deal with 2D vertex data again.

Sharing buffers for 2D and 3D is simple and works OK, but presents a problem when mixing 2D and 3D: `DEPTH_TEST`. 2D screen elements are drawn in front of or behind the 3D elements; we don't want that.

An issue was introduced to try to find a solution for all this problems: [Issue 89](https://github.com/raysan5/raylib/issues/89)

A first idea was adding additional `LINES` and `TRIANGLES` buffers only for 3D, but it was discarded due to the extra complexity and memory usage for raylib. Considering that most of the time raylib users will probably use the library for 2D games and even in the case of using 3D, loaded 3D models are stored in **separate static buffers**. No need to overload the library with a new set of dynamic buffers.

After some thinking, one solution was introduced, probably not the best one, but it gives a balance between 2D and 3D drawing, depth testing and blending.

**SOLUTION:** Disable `DEPTH_TEST` for 2D and enable it only for 3D. The problem with this solution (a part of not having `DEPTH_TEST` on 2D) was the blending between elements from different buffers (i.e. `TRIANGLES` and `QUADS`). It was partially solved moving some commonly-used 2D elements (`Circle`, `Rectangle`, `Lines`) to `QUADS` usage. That way, `DEPTH_TEST` and blending works OK with those elements. (Probably a final solution would be moving all the elements drawing to a single buffer type: QUADS).