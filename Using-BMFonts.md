raylib supports AngelCode sprite fonts (BMFonts).

BMFonts can be created using different programs. The better one is the official Bitmap Font Generator from [AngelCode.com](http://www.angelcode.com/products/bmfont/). Just download and install.

**Recommended BMFont settings**

![BMFont recommended font settings](http://www.raylib.com/images/BMFont_font_settings.png) ![BMFont recommended export options](http://www.raylib.com/images/BMFont_export_options.png)

Some notes:
 - Recommended to select only characters from `SPACE (32)` to `ÿ (255)`, anything above this limit will be loaded but it will fail on rendering, improved support for Unicode characters is on the TODO list.
 - Be careful when changing font size in BMFont `Font Settings`, texture size in `Export Settings` also requires to change to fit new size (BMFont does not change this automatically), `Visualize` font on BMFont to make sure everything is ok.

 - raylib requires the .png file together with the .fnt text file (not binary) placed in the same folder.


Alternatively, it's possible to use the web tool [Littera](http://kvazars.com/littera/) to generate BMFonts but due to some differences with original software, some exported fonts could not work with raylib.

The following export options for Littera are recommended:

* Included glyphs: Presets: **Basic**
* Format: **Text (.fnt)**
* Canvas size: 256 or 512 or 1024
* Power of 2 enabled
* Pack method: **7**

Enjoy using SpriteFonts with raylib!