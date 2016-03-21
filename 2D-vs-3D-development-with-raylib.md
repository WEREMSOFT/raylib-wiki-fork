raylib was primary conceived for 2D game programming but with some basic 3D support. Every new raylib versión expanded that 3D support including shaders and postprocessing effects. Dealing with 2D and 3D in an easy way supposed some technical trades that maybe were not the perfect solution for 2D neither 3D but work very well for both systems coexisting.

####Three different buffers

raylib creates a bunch of vertex buffers (VBO) at initialization. Those buffers are used independently for 2D and 3D drawing and are updated in a frame-basis. Buffers size has been adjusted for a good performance. Depending on the information stored in each buffers-group, we have:

LINES VBOs (attributes: [position, color]) - Store information for lines drawing, two vertex per line.
TRIANGLES VBOs (attributes: [position, color]) - Store information for triangles, three vertex per triangle.
QUADS VBOs (attributes: [position, texcoords, color] + indices) - Store information for quads, four vertex per quad + 6 index per quad.

Buffers are created at `InitWindow()`, vertex data is stored into buffers with every `Draw*()` function and buffers are processed for drawing on `EndDrawing()`.

Depending on the element drawn, vertex are stored in LINES, TRIANGLES or QUADS buffers. That could suppose a problem because, despite including z-depth information, buffers are processed in an specific order: (1)LINES -> (2)TRIANGLES -> (3)QUADS

If no DEPTH_TEST is enabled, QUADS-based shapes will be always drawn in front of TRIANGLES and LINES; if DEPTH_TEST is enabled, drawing is resolved ok thanks to the depth-buffer... until we use semi-transparent shapes.

####Alpha Blending

Due to the fact that the three buffer types are processed independently, LINES fragments are generated before TRIANGLE fragments that are also generated before QUADS fragments. For that reason, despite having DEPTH_TEST working ok, if we try draw a semi-transparent TRIANGLES-based shape in front of a QUAD-based shape, QUAD-based shape fragments are not ready yet to be blended with TRIANGLES-based shape. That's a problem.

####2D and 3D mixing

In addition to the above problem, we have 2D vs 3D mixing. We use the same buffers for 2D drawing and 3D drawing; when changing to 3D mode (`Begin3dMode()`) buffers are processed and reseted to deal with 3D vertex data. Same way, when switching back to 2D mode (`End3dMode()`) buffers are processed and reseted to deal with 2D vertex data.

Sharing buffers for 2D and 3D is simple and works ok but presents a problem when mixing 2D and 3D: DEPTH_TEST
