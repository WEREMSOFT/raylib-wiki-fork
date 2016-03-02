raylib 1.4 supports AngelCode sprite fonts (BMFonts).

BMFonts can be created using different programs. The better one is the official Bitmap Font Generator from [AngelCode.com](http://www.angelcode.com/products/bmfont/). Just download and install.

raylib requires the .png file together with the .fnt text file (not binary) placed in the same folder.

Alternatively, it's possible to use the web tool [Littera](http://kvazars.com/littera/) to generate BMFonts but due to some differences with original software, some exported fonts could not work with raylib.

The following export options for Littera are recommended:

* Included glyphs: Presets: **Basic**
* Format: **Text (.fnt)**
* Canvas size: 256 or 512 or 1024
* Power of 2 enabled
* Pack method: **7**

Enjoy using SpriteFonts with raylib!