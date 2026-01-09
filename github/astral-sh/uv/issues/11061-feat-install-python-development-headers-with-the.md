---
number: 11061
title: "feat: install python development headers with the virtual environment"
type: issue
state: closed
author: ZeyadMoustafaKamal
labels:
  - question
assignees: []
created_at: 2025-01-29T13:36:04Z
updated_at: 2025-01-29T16:17:24Z
url: https://github.com/astral-sh/uv/issues/11061
synced_at: 2026-01-07T13:12:18-06:00
---

# feat: install python development headers with the virtual environment

---

_Issue opened by @ZeyadMoustafaKamal on 2025-01-29 13:36_

### Summary

I have Python 3.13 installed system-wide and initially created a virtual environment with this version. However, some of the required libraries depend on Python 3.12, so I set up another virtual environment using Python 3.12.

When trying to build `pyobject`, I ran into an issue because some of its dependencies require C extensions compiled with `gcc`. The problem is that the build process fails to find the Python.h header file. Strangely, it doesn’t seem to use the headers from Python 3.13—even though I assumed they should be the same.

Wouldn't it make sense to install Python headers automatically for each virtual environment? Or at least provide an option to enable this via a command-line argument?

### Example

_No response_

---

_Label `enhancement` added by @ZeyadMoustafaKamal on 2025-01-29 13:36_

---

_Comment by @zanieb on 2025-01-29 14:14_

Are you sure you're on our latest Python versions?

e.g., see https://github.com/astral-sh/uv/issues/10558#issuecomment-2588501898


---

_Label `enhancement` removed by @zanieb on 2025-01-29 14:14_

---

_Label `question` added by @zanieb on 2025-01-29 14:14_

---

_Comment by @ZeyadMoustafaKamal on 2025-01-29 15:06_

The same issue

```

Gradience on  feat/setup.py:main [!⇡] is 󰏗 v0.8.0-beta2 via  v3.12.8 (.venv) 
❯ uv python install --reinstall 3.12
Installed Python 3.12.8 in 9.81s
 ~ cpython-3.12.8-linux-x86_64-gnu

Gradience on  feat/setup.py:main [!⇡] is 󰏗 v0.8.0-beta2 via  v3.12.8 (.venv) took 9s 
❯ uv pip install pygobject
Using Python 3.12.8 environment at: .fvenv
Resolved 2 packages in 2ms
  × Failed to build `pycairo==1.27.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `mesonpy.build_wheel` failed (exit status: 1)

      [stdout]
      + meson setup /home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src
      /home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src/.mesonpy-0bhhx9_9 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md -Dwheel=true
      -Dtests=false --native-file=/home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src/.mesonpy-0bhhx9_9/meson-python-native-file.ini
      The Meson build system
      Version: 1.7.0
      Source dir: /home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src
      Build dir: /home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src/.mesonpy-0bhhx9_9
      Build type: native build
      Project name: pycairo
      Project version: 1.27.0
      C compiler for the host machine: ccache cc (gcc 14.2.1 "cc (GCC) 14.2.1 20240910")
      C linker for the host machine: cc ld.bfd 2.43.1
      Host machine cpu family: x86_64
      Host machine cpu: x86_64
      Program python3 found: YES (/home/zeyad/.cache/uv/builds-v0/.tmpH79L2J/bin/python)
      Compiler for C supports arguments -Wall: YES
      Compiler for C supports arguments -Warray-bounds: YES
      Compiler for C supports arguments -Wcast-align: YES
      Compiler for C supports arguments -Wconversion: YES
      Compiler for C supports arguments -Wextra: YES
      Compiler for C supports arguments -Wformat=2: YES
      Compiler for C supports arguments -Wformat-nonliteral: YES
      Compiler for C supports arguments -Wformat-security: YES
      Compiler for C supports arguments -Wimplicit-function-declaration: YES
      Compiler for C supports arguments -Winit-self: YES
      Compiler for C supports arguments -Winline: YES
      Compiler for C supports arguments -Wmissing-format-attribute: YES
      Compiler for C supports arguments -Wmissing-noreturn: YES
      Compiler for C supports arguments -Wnested-externs: YES
      Compiler for C supports arguments -Wold-style-definition: YES
      Compiler for C supports arguments -Wpacked: YES
      Compiler for C supports arguments -Wpointer-arith: YES
      Compiler for C supports arguments -Wreturn-type: YES
      Compiler for C supports arguments -Wshadow: YES
      Compiler for C supports arguments -Wsign-compare: YES
      Compiler for C supports arguments -Wstrict-aliasing: YES
      Compiler for C supports arguments -Wundef: YES
      Compiler for C supports arguments -Wunused-but-set-variable: YES
      Compiler for C supports arguments -Wswitch-default: YES
      Compiler for C supports arguments -Wno-missing-field-initializers: YES
      Compiler for C supports arguments -Wno-unused-parameter: YES
      Compiler for C supports arguments -fno-strict-aliasing: YES
      Compiler for C supports arguments -fvisibility=hidden: YES
      Found pkg-config: YES (/usr/bin/pkg-config) 2.3.0
      Run-time dependency cairo found: YES 1.18.2
      Run-time dependency python found: YES 3.12
      Build targets in project: 4

      pycairo 1.27.0

        User defined options
          Native files: /home/zeyad/.cache/uv/sdists-v6/pypi/pycairo/1.27.0/fQy7pxADqBmjESCne7weF/src/.mesonpy-0bhhx9_9/meson-python-native-file.ini
          b_ndebug    : if-release
          b_vscrt     : md
          buildtype   : release
          tests       : false
          wheel       : true

      Found ninja-1.12.1 at /usr/bin/ninja
      + /usr/bin/ninja
      [1/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/path.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/path.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/path.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/path.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/path.c.o -c ../cairo/path.c
      ../cairo/path.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [2/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/cairomodule.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/cairomodule.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/cairomodule.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/cairomodule.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/cairomodule.c.o -c ../cairo/cairomodule.c
      ../cairo/cairomodule.c:33:10: fatal error: Python.h: No such file or directory
         33 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [3/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/enums.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/enums.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/enums.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/enums.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/enums.c.o -c ../cairo/enums.c
      ../cairo/enums.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [4/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/context.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/context.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/context.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/context.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/context.c.o -c ../cairo/context.c
      ../cairo/context.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [5/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/pattern.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/pattern.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/pattern.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/pattern.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/pattern.c.o -c ../cairo/pattern.c
      ../cairo/pattern.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [6/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/error.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/error.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/error.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/error.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/error.c.o -c ../cairo/error.c
      ../cairo/error.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [7/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/device.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/device.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/device.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/device.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/device.c.o -c ../cairo/device.c
      ../cairo/device.c:30:10: fatal error: Python.h: No such file or directory
         30 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [8/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/font.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/font.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/font.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/font.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/font.c.o -c ../cairo/font.c
      ../cairo/font.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [9/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/misc.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/misc.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/misc.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/misc.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/misc.c.o -c ../cairo/misc.c
      ../cairo/misc.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [10/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/matrix.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/matrix.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/matrix.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/matrix.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/matrix.c.o -c ../cairo/matrix.c
      ../cairo/matrix.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [11/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/surface.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/surface.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/surface.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/surface.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/surface.c.o -c ../cairo/surface.c
      ../cairo/surface.c:33:10: fatal error: Python.h: No such file or directory
         33 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [12/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/bufferproxy.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/bufferproxy.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/bufferproxy.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/bufferproxy.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/bufferproxy.c.o -c ../cairo/bufferproxy.c
      ../cairo/bufferproxy.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [13/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/glyph.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/glyph.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/glyph.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/glyph.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/glyph.c.o -c ../cairo/glyph.c
      ../cairo/glyph.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [14/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textcluster.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textcluster.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textcluster.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textcluster.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textcluster.c.o -c ../cairo/textcluster.c
      ../cairo/textcluster.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [15/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textextents.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textextents.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textextents.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textextents.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/textextents.c.o -c ../cairo/textextents.c
      ../cairo/textextents.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [16/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/region.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/region.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/region.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/region.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/region.c.o -c ../cairo/region.c
      ../cairo/region.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [17/21] Compiling C object cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/rectangle.c.o
      FAILED: cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/rectangle.c.o
      ccache cc -Icairo/_cairo.cpython-312-x86_64-linux-gnu.so.p -Icairo -I../cairo -I/usr/include/cairo -I/usr/include/freetype2 -I/usr/include/libpng16
      -I/usr/include/harfbuzz -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/include/sysprof-6 -I/usr/include/pixman-1 -I/install/include/python3.12
      -fvisibility=hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -O3 -fPIC -pthread -DPYCAIRO_VERSION_MAJOR=1
      -DPYCAIRO_VERSION_MINOR=27 -DPYCAIRO_VERSION_MICRO=0 -Wall -Warray-bounds -Wcast-align -Wconversion -Wextra -Wformat=2 -Wformat-nonliteral -Wformat-security
      -Wimplicit-function-declaration -Winit-self -Winline -Wmissing-format-attribute -Wmissing-noreturn -Wnested-externs -Wold-style-definition -Wpacked
      -Wpointer-arith -Wreturn-type -Wshadow -Wsign-compare -Wstrict-aliasing -Wundef -Wunused-but-set-variable -Wswitch-default -Wno-missing-field-initializers
      -Wno-unused-parameter -fno-strict-aliasing -fvisibility=hidden -MD -MQ cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/rectangle.c.o -MF
      cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/rectangle.c.o.d -o cairo/_cairo.cpython-312-x86_64-linux-gnu.so.p/rectangle.c.o -c ../cairo/rectangle.c
      ../cairo/rectangle.c:32:10: fatal error: Python.h: No such file or directory
         32 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      [18/21] Copying file cairo/__init__.py
      ninja: build stopped: subcommand failed.

      hint: This usually indicates a problem with the package or the build environment.
  help: `pycairo` (v1.27.0) was included because `pygobject` (v3.50.0) depends on `pycairo`
```

---

_Referenced in [ZeyadMoustafaKamal/Gradience#1](../../ZeyadMoustafaKamal/Gradience/pulls/1.md) on 2025-01-29 15:17_

---

_Comment by @zanieb on 2025-01-29 15:54_

You can see includes like `-I/install/include/python3.12` which should be entirely removed on the latest uv version. What version are you using..?

---

_Comment by @zanieb on 2025-01-29 15:56_

Please provide the information described in https://github.com/astral-sh/uv/issues/9452 e.g., verbose logs, versions, and platforms.

---

_Comment by @ZeyadMoustafaKamal on 2025-01-29 16:17_

Yeah after I updated `uv` everything works fine now. Thanks.

---

_Closed by @ZeyadMoustafaKamal on 2025-01-29 16:17_

---

_Referenced in [laughingman7743/PyAthena#616](../../laughingman7743/PyAthena/pulls/616.md) on 2025-10-18 07:51_

---

_Referenced in [laughingman7743/PyAthena#615](../../laughingman7743/PyAthena/issues/615.md) on 2025-10-18 07:51_

---
