raylib 1.4 supports AngelCode sprite fonts (BMFonts).

BMFonts can be created using different programs. The better one is the official Bitmap Font Generator from [AngelCode.com](http://www.angelcode.com/products/bmfont/). Just download and install.

When using BMFont, these settings proved necessary:
 - Select only the 95 characters from `SPACE (32)` to `~ (126)` -- anything else triggers the *"unordered chars data"* error (well, actually, you can leave out multiple contiguous characters at the end of that sequence....). BMFont doesn't do this by default or have a built-in way to do it, so you have to do it manually.
 - Change `Bit Depth` to 32 in `Export Settings` from the 8 bit default -- otherwise raylib renders all glyphs as solid rectangles.
 - Also, be careful with the following crash bug: If you change the font size in BMFont `Font Settings`, but forget to increase the texture size in `Export Settings` to match (BMFont does not do this automatically), raylib crashes when you try to load that font.

raylib requires the .png file together with the .fnt text file (not binary) placed in the same folder.

Alternatively, it's possible to use the web tool [Littera](http://kvazars.com/littera/) to generate BMFonts but due to some differences with original software, some exported fonts could not work with raylib.

The following export options for Littera are recommended:

* Included glyphs: Presets: **Basic**
* Format: **Text (.fnt)**
* Canvas size: 256 or 512 or 1024
* Power of 2 enabled
* Pack method: **7**

Enjoy using SpriteFonts with raylib!