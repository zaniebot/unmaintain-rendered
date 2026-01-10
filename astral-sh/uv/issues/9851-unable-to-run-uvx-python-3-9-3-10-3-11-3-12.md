---
number: 9851
title: "Unable to run `uvx --python [3.9|3.10|3.11|3.12] textual-paint`"
type: issue
state: closed
author: BioGeek
labels:
  - question
assignees: []
created_at: 2024-12-12T20:50:11Z
updated_at: 2025-02-18T15:18:32Z
url: https://github.com/astral-sh/uv/issues/9851
synced_at: 2026-01-10T01:24:46Z
---

# Unable to run `uvx --python [3.9|3.10|3.11|3.12] textual-paint`

---

_Issue opened by @BioGeek on 2024-12-12 20:50_

I am unable to get [uvx](https://docs.astral.sh/uv/guides/tools/#running-tools) working with [textual-paint ](https://github.com/1j01/textual-paint).

I first tried `uvx --python 3.9 textual-paint`:

```
$ uvx --python 3.9 textual-paint
Installed 19 packages in 13ms
Traceback (most recent call last):
  File "/home/j-vangoey/.cache/uv/archive-v0/TkQdylBcXlZcpfEIv5uB1/bin/textual-paint", line 7, in <module>
    from textual_paint.paint import main
  File "/home/j-vangoey/.cache/uv/archive-v0/TkQdylBcXlZcpfEIv5uB1/lib/python3.9/site-packages/textual_paint/paint.py", line 35, in <module>
    from textual_paint.action import Action
  File "/home/j-vangoey/.cache/uv/archive-v0/TkQdylBcXlZcpfEIv5uB1/lib/python3.9/site-packages/textual_paint/action.py", line 5, in <module>
    from textual_paint.ansi_art_document import AnsiArtDocument
  File "/home/j-vangoey/.cache/uv/archive-v0/TkQdylBcXlZcpfEIv5uB1/lib/python3.9/site-packages/textual_paint/ansi_art_document.py", line 583
    match instruction.region:
          ^
SyntaxError: invalid syntax
```
After looking at the `textual-paint` source code [I see in `setyp.cfg`: `python_requires = >=3.10,<3.13`](https://github.com/1j01/textual-paint/blob/d61de649a6a3a660d2b024e4259d99acbd45116b/setup.cfg#L58). So preferably `uvx` should be able to parse that and return a warning that `textual-paint` is not compatible with `python 3.9` instead of throwing a `SyntaxError`

Next up,  `uvx --python 3.10 textual-paint` and `uvx --python 3.11 textual-paint` both fail with the same `ModuleNotFoundError`. This seems related to `pyfiglet` where I found a few related issues (https://github.com/pwaller/pyfiglet/issues/113 https://github.com/pwaller/pyfiglet/issues/124), but which [should be fixed in v1.0.0 of `pyfiglet`](https://github.com/pwaller/pyfiglet/issues/124#issuecomment-1712744359).

```
$ uvx --python 3.10 textual-paint
Installed 19 packages in 31ms
Traceback (most recent call last):
  File "/home/j-vangoey/.cache/uv/archive-v0/R8oliKzRfOI5e7eVFwK9U/bin/textual-paint", line 7, in <module>
    from textual_paint.paint import main
  File "/home/j-vangoey/.cache/uv/archive-v0/R8oliKzRfOI5e7eVFwK9U/lib/python3.10/site-packages/textual_paint/paint.py", line 44, in <module>
    from textual_paint.canvas import Canvas
  File "/home/j-vangoey/.cache/uv/archive-v0/R8oliKzRfOI5e7eVFwK9U/lib/python3.10/site-packages/textual_paint/canvas.py", line 17, in <module>
    from textual_paint.meta_glyph_font import largest_font_that_fits
  File "/home/j-vangoey/.cache/uv/archive-v0/R8oliKzRfOI5e7eVFwK9U/lib/python3.10/site-packages/textual_paint/meta_glyph_font.py", line 5, in <module>
    from pyfiglet import Figlet, FigletFont  # type: ignore
  File "/home/j-vangoey/.cache/uv/archive-v0/R8oliKzRfOI5e7eVFwK9U/lib/python3.10/site-packages/pyfiglet/__init__.py", line 11, in <module>
    import pkg_resources
ModuleNotFoundError: No module named 'pkg_resources'
```

```
$ uvx --python 3.11 textual-paint
Installed 19 packages in 16ms
Traceback (most recent call last):
  File "/home/j-vangoey/.cache/uv/archive-v0/7odsptFFgOts7-Yke6ZXO/bin/textual-paint", line 7, in <module>
    from textual_paint.paint import main
  File "/home/j-vangoey/.cache/uv/archive-v0/7odsptFFgOts7-Yke6ZXO/lib/python3.11/site-packages/textual_paint/paint.py", line 44, in <module>
    from textual_paint.canvas import Canvas
  File "/home/j-vangoey/.cache/uv/archive-v0/7odsptFFgOts7-Yke6ZXO/lib/python3.11/site-packages/textual_paint/canvas.py", line 17, in <module>
    from textual_paint.meta_glyph_font import largest_font_that_fits
  File "/home/j-vangoey/.cache/uv/archive-v0/7odsptFFgOts7-Yke6ZXO/lib/python3.11/site-packages/textual_paint/meta_glyph_font.py", line 5, in <module>
    from pyfiglet import Figlet, FigletFont  # type: ignore
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/j-vangoey/.cache/uv/archive-v0/7odsptFFgOts7-Yke6ZXO/lib/python3.11/site-packages/pyfiglet/__init__.py", line 11, in <module>
    import pkg_resources
ModuleNotFoundError: No module named 'pkg_resources'
```
And finally `uvx --python 3.12 textual-paint` had a problem with installing Pillow

```
$ uvx --python 3.12 textual-paint
   Built pyperclip==1.8.2
  × Failed to download and build `pillow==9.5.0`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/features.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ExifTags.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/BufrStubImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/Hdf5StubImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/QoiImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/_util.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/TiffTags.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/WmfImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageOps.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PcfFontFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageStat.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/Image.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/_version.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/JpegImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/EpsImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/IptcImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/DdsImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/CurImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GbrImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/_binary.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageMorph.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PngImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PdfParser.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/BdfFontFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageMode.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FpxImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/__init__.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PcxImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageFont.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageShow.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PixarImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageTk.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImagePalette.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/SgiImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/BlpImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/_tkinter_finder.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FliImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FontFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PsdImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GdImageFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/BmpImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageMath.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageCms.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PaletteFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageColor.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/XbmImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageGrab.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImtImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/TiffImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageEnhance.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/SunImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageTransform.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageDraw2.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PalmImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/TarIO.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/IcoImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/Jpeg2KImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PSDraw.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PyAccess.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PdfImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/MpoImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/SpiderImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/XVThumbImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageWin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/IcnsImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FtexImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GifImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/WebPImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/JpegPresets.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FitsImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImagePath.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/WalImageFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/XpmImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/MicImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageQt.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/McIdasImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/_deprecate.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/MspImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PpmImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageSequence.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageChops.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/FitsStubImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/__main__.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageFilter.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GimpGradientFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/TgaImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GribStubImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/PcdImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/MpegImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ContainerIO.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/DcxImagePlugin.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/ImageDraw.py -> build/lib.linux-x86_64-cpython-312/PIL
      copying src/PIL/GimpPaletteFile.py -> build/lib.linux-x86_64-cpython-312/PIL
      running egg_info
      writing src/Pillow.egg-info/PKG-INFO
      writing dependency_links to src/Pillow.egg-info/dependency_links.txt
      writing requirements to src/Pillow.egg-info/requires.txt
      writing top-level names to src/Pillow.egg-info/top_level.txt
      reading manifest file 'src/Pillow.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'LICENSE'
      writing manifest file 'src/Pillow.egg-info/SOURCES.txt'
      running build_ext

      [stderr]
      warning: no files found matching '*.c'
      warning: no files found matching '*.h'
      warning: no files found matching '*.sh'
      warning: no files found matching '*.txt'
      warning: no previously-included files found matching '.appveyor.yml'
      warning: no previously-included files found matching '.clang-format'
      warning: no previously-included files found matching '.coveragerc'
      warning: no previously-included files found matching '.editorconfig'
      warning: no previously-included files found matching '.readthedocs.yml'
      warning: no previously-included files found matching 'codecov.yml'
      warning: no previously-included files found matching 'renovate.json'
      warning: no previously-included files matching '.git*' found anywhere in distribution
      warning: no previously-included files matching '*.pyc' found anywhere in distribution
      warning: no previously-included files matching '*.so' found anywhere in distribution
      no previously-included directories found matching '.ci'


      The headers or library files could not be found for jpeg,
      a required dependency when compiling Pillow from source.

      Please see the install instructions at:
         https://pillow.readthedocs.io/en/latest/installation.html

      Traceback (most recent call last):
        File "<string>", line 993, in <module>
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/__init__.py", line 117, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/core.py", line 183, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/core.py", line 199, in run_commands
          dist.run_commands()
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/dist.py", line 954, in run_commands
          self.run_command(cmd)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/dist.py", line 995, in run_command
          super().run_command(command)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
          cmd_obj.run()
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/command/bdist_wheel.py", line 381, in run
          self.run_command("build")
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
          self.distribution.run_command(command)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/dist.py", line 995, in run_command
          super().run_command(command)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
          cmd_obj.run()
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/command/build.py", line 135, in run
          self.run_command(cmd_name)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
          self.distribution.run_command(command)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/dist.py", line 995, in run_command
          super().run_command(command)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
          cmd_obj.run()
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/command/build_ext.py", line 99, in run
          _build_ext.run(self)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/_distutils/command/build_ext.py", line 359, in run
          self.build_extensions()
        File "<string>", line 809, in build_extensions
      RequiredDependencyException: jpeg

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/build_meta.py", line 435, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/build_meta.py", line 426, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/build_meta.py", line 407, in _build_with_temp_dir
          self.run_setup()
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/home/j-vangoey/.cache/uv/builds-v0/.tmpaZdtRn/lib/python3.12/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 1010, in <module>
      RequiredDependencyException:

      The headers or library files could not be found for jpeg,
      a required dependency when compiling Pillow from source.

      Please see the install instructions at:
         https://pillow.readthedocs.io/en/latest/installation.html



  help: `pillow` (v9.5.0) was included because `textual-paint` (v0.4.0) depends on `pillow`
```

## System info:

OS: Ubuntu 22.04.4 LTS x86_64 
uv version: 0.5.6



---

_Comment by @zanieb on 2024-12-12 21:17_

That's an impressive amount of problems! Sorry you ran into these. We'll need to follow-up on a few thnings here.

---

_Assigned to @zanieb by @zanieb on 2024-12-12 21:17_

---

_Label `bug` added by @zanieb on 2024-12-12 21:17_

---

_Referenced in [1j01/textual-paint#9](../../1j01/textual-paint/pulls/9.md) on 2024-12-12 22:03_

---

_Comment by @zanieb on 2025-02-18 15:18_

Regarding the Python version constraint, it looks like this is missing from their published package

Here's the wheel metadata, which does not include a `Requires-Python` field
https://inspector.pypi.io/project/textual-paint/0.4.0/packages/71/ca/7929fa18b01403682da3180fedfcfeebabaa5a7e6ad773821a4e8cc7da64/textual_paint-0.4.0-py3-none-any.whl/textual_paint-0.4.0.dist-info/METADATA

for an example of correct metadata, see
https://inspector.pypi.io/project/anyio/4.8.0/packages/46/eb/e7f063ad1fec6b3178a3cd82d1a3c4de82cccf283fc42746168188e1cdd5/anyio-4.8.0-py3-none-any.whl/anyio-4.8.0.dist-info/METADATA#line.23 

Unfortunately that's a problem with the package or setuptools, not something uv can fix.

Regarding the `pkg_resources` error, unfortunately, again that's a problem with another package and we can't do anything there except perhaps provide a hint about why that error could happen?

The `pillow` failure seems about right — the error says why it happens and links to their installation documentation. We can't manage system dependencies like that. See https://docs.astral.sh/uv/reference/troubleshooting/build-failures/

Sorry I don't have better answers here, but I don't see any bugs in uv.

---

_Closed by @zanieb on 2025-02-18 15:18_

---

_Label `bug` removed by @zanieb on 2025-02-18 15:18_

---

_Label `question` added by @zanieb on 2025-02-18 15:18_

---

_Unassigned @zanieb by @zanieb on 2025-02-18 15:18_

---
