raylib was primary conceived for 2D game programming but with some basic 3D support. Every new raylib versión expanded that 3D support including shaders and postprocessing effects. Dealing with 2D and 3D in an easy way supposed some technical trades that maybe were not the perfect solution for 2D neither 3D but work very well for both systems coexisting.

Three different buffers

raylib creates a bunch of vertex buffers (VBO) at initialization. Those buffers are used independently for 2D and 3D drawing and are updated in a frame-basis. Buffers size has been adjusted for a good performance. Depending on the information stored in each buffers-group, we have:

LINES VBOs (attributes: [position, color]) - Store information for lines drawing, two vertex per line.
TRIANGLES VBOs (attributes: [position, color]) - Store information for triangles, three vertex per triangle.
QUADS VBOs (attributes: [position, texcoords, color] + indices) - Store information for quads, four vertex per quad + 6 index per quad.

Depending on the element drawn, vertex are stored in LINES, TRIANGLES or QUADS buffers. That could suppose a problem because, despite including z-depth informatio, buffers are processed in an specific order: (1)LINES -> (2)TRIANGLES -> (3)QUADS

If no DEPTH_TEST is enabled, QUADS-based shapes will be always drawn in front of TRIANGLES and LINES; if DEPTH_TEST is enabled, drawing is resolved ok thanks to the depth-buffer... until we use semi-transparent shapes.

Alpha Blending

Due to the fact that the three buffer types are processed independently, LINES fragments are generated before TRIANGLE fragments that are also generated before QUADS fragments. For that reason, despite having DEPTH_TEST working ok, if we try draw a semi-transparent TRIANGLES-based shape in front of a QUAD-based shape, QUAD-based shape fragments are not ready yet to be blended with TRIANGLES-based shape. That's a problem.


