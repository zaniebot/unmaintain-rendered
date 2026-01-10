---
number: 10558
title: "Failing to compile numpy from source against a uv-managed interpreter (`Cannot compile `Python.h`.`)"
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2025-01-13T08:36:25Z
updated_at: 2025-01-17T05:18:34Z
url: https://github.com/astral-sh/uv/issues/10558
synced_at: 2026-01-10T01:24:55Z
---

# Failing to compile numpy from source against a uv-managed interpreter (`Cannot compile `Python.h`.`)

---

_Issue opened by @neutrinoceros on 2025-01-13 08:36_

I'm not able to build numpy from source against a uv-managed Python:

```
../meson.build:44:2: ERROR: Problem encountered: Cannot compile `Python.h`. Perhaps you need to install python-dev|python-devel
```
this seems similar to #10185, which was transferred from https://github.com/astral-sh/python-build-standalone, so I'm opening this ticket directly in uv's repo. This issue was already present with uv 0.5.13 and appears to reproduce with 0.5.18

setup:
```
$ git clone https://github.com/numpy/numpy
$ cd numpy
$ git submodule update --init
```
then
```
$ uv build
Building source distribution...
+ /Users/clm/.cache/uv/builds-v0/.tmpcUq9Nd/bin/python /private/tmp/numpy/vendored-meson/meson/meson.py setup /private/tmp/numpy /private/tmp/numpy/.mesonpy-xqqxtm9n -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=/private/tmp/numpy/.mesonpy-xqqxtm9n/meson-python-native-file.ini
The Meson build system
Version: 1.5.2
Source dir: /private/tmp/numpy
Build dir: /private/tmp/numpy/.mesonpy-xqqxtm9n
Build type: native build
Project name: NumPy
Project version: 2.3.0.dev0+git20241226.a07c6c5
C compiler for the host machine: cc (clang 16.0.0 "Apple clang version 16.0.0 (clang-1600.0.26.6)")
C linker for the host machine: cc ld64 1115.7.3
C++ compiler for the host machine: c++ (clang 16.0.0 "Apple clang version 16.0.0 (clang-1600.0.26.6)")
C++ linker for the host machine: c++ ld64 1115.7.3
Cython compiler for the host machine: cython (cython 3.0.11)
Host machine cpu family: aarch64
Host machine cpu: aarch64
Program python found: YES (/Users/clm/.cache/uv/builds-v0/.tmpcUq9Nd/bin/python)
Found pkg-config: YES (/opt/homebrew/bin/pkg-config) 2.3.0
Run-time dependency python found: YES 3.13
Has header "Python.h" with dependency python-3.13: NO

../meson.build:44:2: ERROR: Problem encountered: Cannot compile `Python.h`. Perhaps you need to install python-dev|python-devel

A full log can be found at /private/tmp/numpy/.mesonpy-xqqxtm9n/meson-logs/meson-log.txt
  × Failed to build `/private/tmp/numpy`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `mesonpy.build_sdist` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

A shorter path to the same error
```
$ uv venv
$ uv pip install git+https://github.com/numpy/numpy
```




---

_Referenced in [astral-sh/uv#10185](../../astral-sh/uv/issues/10185.md) on 2025-01-13 08:36_

---

_Comment by @zanieb on 2025-01-13 18:46_

The header file is available, e.g., on my machine at `~/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13/Python.h`

I wonder how their configure script seaches for it?

---

_Label `question` added by @zanieb on 2025-01-13 18:46_

---

_Assigned to @zanieb by @zanieb on 2025-01-13 18:46_

---

_Comment by @oscarbenjamin on 2025-01-14 00:06_

I've just run into what I assume is the same issue trying to build python-flint with a uv-installed Python. I have made a simple demonstration repo for a project that has meson as build system and builds a Cython extension module:

https://github.com/oscarbenjamin/meson_simple

It builds fine with a system Python or pyenv Python but not with a uv python.

Steps:
```
git clone https://github.com/oscarbenjamin/meson_simple
cd meson_simple
uv venv
uv pip install -r requirements-dev.txt
source .venv/bin/activate
meson setup build --wipe
meson compile -C build
spin test
```

I don't know if this is a new issue because I have not previously tried to build anything like this with a uv-installed Python.

This is the meson configure output:
```console
$ meson setup build --wipe
The Meson build system
Version: 1.6.1
Source dir: /stuff/current/active/mesontest
Build dir: /stuff/current/active/mesontest/build
Build type: native build
Project name: meson-test
Project version: undefined
C compiler for the host machine: cc (gcc 11.4.0 "cc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0")
C linker for the host machine: cc ld.bfd 2.38
Cython compiler for the host machine: cython (cython 3.0.11)
Host machine cpu family: x86_64
Host machine cpu: x86_64
Program python3 found: YES (/stuff/current/active/mesontest/.venv/bin/python3)
Found pkg-config: YES (/usr/bin/pkg-config) 0.29.2
Run-time dependency python found: YES 3.13
Build targets in project: 1

Found ninja-1.10.1 at /usr/bin/ninja
INFO: autodetecting backend as ninja
INFO: calculating backend command to run: /usr/bin/ninja -C /stuff/current/active/mesontest/build
```
The compile step fails with:
```console
$ meson compile -C build
ninja: Entering directory `/stuff/current/active/mesontest/build'
[2/3] Compiling C object meson_test/_meson_test.c....p/meson-generated_meson_test__meson_test.pyx.c.o
FAILED: meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o 
cc -Imeson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p -Imeson_test -I../meson_test -I/install/include/python3.13 -fvisibility=hidden -fdiagnostics-color=always -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -std=c99 -O2 -g -fPIC -MD -MQ meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o -MF meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o.d -o meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o -c meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson_test/_meson_test.pyx.c
meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson_test/_meson_test.pyx.c:16:10: fatal error: Python.h: No such file or directory
   16 | #include "Python.h"
      |          ^~~~~~~~~~
compilation terminated.
ninja: build stopped: subcommand failed.
```
The specific compiler command wrapped is:
```
cc -Imeson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p -Imeson_test
-I../meson_test -I/install/include/python3.13 -fvisibility=hidden
-fdiagnostics-color=always -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -std=c99
-O2 -g -fPIC -MD -MQ
meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o
-MF
meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o.d
-o
meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o
-c
meson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p/meson_test/_meson_test.pyx.c
```
Running with a venv off of a system Python I get this command instead:
```
cc -Imeson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p -Imeson_test
-I../meson_test -I/usr/include/python3.10
-I/usr/include/x86_64-linux-gnu/python3.10 -fvisibility=hidden
-fdiagnostics-color=always -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -std=c99
-O2 -g -fPIC -MD -MQ
meson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o
-MF
meson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o.d
-o
meson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p/meson-generated_meson_test__meson_test.pyx.c.o
-c
meson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p/meson_test/_meson_test.pyx.c
```
Separating only the include paths it is:
```bash
# uv python
-Imeson_test/_meson_test.cpython-313-x86_64-linux-gnu.so.p
-Imeson_test
-I../meson_test
-I/install/include/python3.13

# system python
-Imeson_test/_meson_test.cpython-310-x86_64-linux-gnu.so.p
-Imeson_test
-I../meson_test
-I/usr/include/python3.10
-I/usr/include/x86_64-linux-gnu/python3.10
```
Somehow it seems that meson has been unable to locate the include directories I guess.

---

_Comment by @oscarbenjamin on 2025-01-14 00:19_

Possibly @eli-schwartz could help with understanding how meson identifies the include directories.

---

_Comment by @zanieb on 2025-01-14 00:21_

Thanks for the additional details.

Looks like this comes from https://github.com/mesonbuild/meson/blob/dfb449d099d6f3066ca646af197bc7af85059a86/mesonbuild/dependencies/python.py#L369-L370

I don't really see a difference between the variables referenced

```
❯ $(uv python find --python-preference only-system) -m sysconfig | rg "(INCLUDEPY|include|platinclude) ="
	include = "/opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12/include/python3.12"
	platinclude = "/opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12/include/python3.12"
	CONFINCLUDEPY = "/opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12/include/python3.12"
	INCLUDEPY = "/opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12/include/python3.12"
❯ $(uv python find --python-preference only-managed) -m sysconfig | rg "(INCLUDEPY|include|platinclude) ="
	include = "/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/include/python3.12"
	platinclude = "/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/include/python3.12"
	CONFINCLUDEPY = "/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/include/python3.12"
	INCLUDEPY = "/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/include/python3.12"
```

Can you see the `pkg-config` it uses?

---

_Comment by @zanieb on 2025-01-14 00:22_

@oscarbenjamin your logs show `-I/install/include/python3.13` which is old uv behavior, can you try reinstalling your interpreter? We change those paths at install time now (see #9857)

---

_Comment by @oscarbenjamin on 2025-01-14 00:27_

Oh, actually after updating uv and reinstalling Python it works for the demo:
```
uv self update
uv python install -r 3.12
rm -r .venv
# then it works
```

---

_Comment by @zanieb on 2025-01-14 00:32_

Great to hear!

Yeah the pkg-config file for these look similar too

```
❯ cat /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/pkgconfig/python-3.12.pc
# See: man pkg-config
prefix=/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: Python
Description: Build a C extension for Python
Requires:
Version: 3.12
Libs.private: -ldl  -framework CoreFoundation
Libs: -L${libdir} 
Cflags: -I${includedir}/python3.12%                                                                                                                                                                                                                           

❯ cat /opt/homebrew/opt/python@3.12/lib/pkgconfig/python-3.12.pc
# See: man pkg-config
prefix=/opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: Python
Description: Build a C extension for Python
Requires:
Version: 3.12
Libs.private: -ldl  -framework CoreFoundation
Libs: -L${libdir} 
Cflags: -I${includedir}/python3.12
```

---

_Comment by @zanieb on 2025-01-14 00:33_

@neutrinoceros can you use `uv python install --reinstall <version>` and try again?

---

_Comment by @oscarbenjamin on 2025-01-14 00:36_

I've confirmed I can build python-flint and run the whole test suite. The problems I was seeing were fixed by updating uv, using the updated uv to reinstall Python and then rebuilding the venv.

Thanks @zanieb !

---

_Comment by @eli-schwartz on 2025-01-14 00:52_

Looks like you were able to figure out the critical references we use, and it turns out everything works with current `uv`?

---

_Comment by @oscarbenjamin on 2025-01-14 01:08_

Thanks @eli-schwartz.

> everything works with current `uv`?

Yes in my testing but it would be good for @neutrinoceros to confirm.

I just tried installing numpy from git as described in the OP and it seems to work okay:
```console
$ uv pip install git+https://github.com/numpy/numpy
 Updated https://github.com/numpy/numpy (bbf4836a8)
Resolved 1 package in 34.85s
   Built numpy @ git+https://github.com/numpy/numpy@bbf4836a82d255c2a2161d277abb4b6f5455ca6d
Prepared 1 package in 5m 54s
Installed 1 package in 19ms
 + numpy==2.3.0.dev0 (from git+https://github.com/numpy/numpy@bbf4836a82d255c2a2161d277abb4b6f5455ca6d)
$ python -c 'import numpy; print(numpy.__version__)'
2.3.0.dev0+git20250110.bbf4836
```
If anyone wants to try that one at home then they would first need to install openblas e.g. something like `apt-get libopenblas-dev`.

---

_Comment by @neutrinoceros on 2025-01-14 07:31_

> @neutrinoceros can you use uv python install --reinstall <version> and try again?

Sorry I should have mentioned: I tried this with uv 0.5.14, which if I'm following correctly has the most recent updates in this area, so I didn't think it was necessary to try again with 0.5.18. I just tried now:

```
uv python install 3.13 --reinstall
uv pip install git+https://github.com/numpy/numpy --no-cache --no-config
```
(throwing `--no-cache` and `--no-config` for good measure)
The error persists :/

---

_Comment by @eli-schwartz on 2025-01-14 07:34_

What are the values for you that @zanieb described in https://github.com/astral-sh/uv/issues/10558#issuecomment-2588488334 ?

---

_Comment by @neutrinoceros on 2025-01-14 08:07_

```
❯ $(uv python find --python-preference only-system) -m sysconfig | rg "(INCLUDEPY|include|platinclude)"
	include = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	platinclude = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	CONFIGURE_CPPFLAGS = "-I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	CONFIG_ARGS = "'--prefix=/Users/clm/.pyenv/versions/3.11.11' '--enable-shared' '--libdir=/Users/clm/.pyenv/versions/3.11.11/lib' '--with-openssl=/opt/homebrew/opt/openssl@3' 'PKG_CONFIG_PATH=/opt/homebrew/opt/openssl@3/lib/pkgconfig/:' 'CC=clang' 'LDFLAGS=-L/opt/homebrew/opt/ncurses/lib -L/opt/homebrew/opt/readline/lib -L/opt/homebrew/opt/readline/lib -L/Users/clm/.pyenv/versions/3.11.11/lib -Wl,-rpath,/Users/clm/.pyenv/versions/3.11.11/lib -L/opt/homebrew/lib -Wl,-rpath,/opt/homebrew/lib -L/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib' 'LIBS=-L/Users/clm/.pyenv/versions/3.11.11/lib -Wl,-rpath,/Users/clm/.pyenv/versions/3.11.11/lib -L/opt/homebrew/lib -Wl,-rpath,/opt/homebrew/lib' 'CPPFLAGS=-I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include'"
	CONFINCLUDEDIR = "/Users/clm/.pyenv/versions/3.11.11/include"
	CONFINCLUDEPY = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	CPPFLAGS = "-I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	INCLDIRSTOMAKE = "/Users/clm/.pyenv/versions/3.11.11/include /Users/clm/.pyenv/versions/3.11.11/include /Users/clm/.pyenv/versions/3.11.11/include/python3.11 /Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	INCLUDEDIR = "/Users/clm/.pyenv/versions/3.11.11/include"
	INCLUDEPY = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	LIBEXPAT_CFLAGS = "-I./Modules/expat -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	LIBMPDEC_CFLAGS = "-I./Modules/_decimal/libmpdec -DUNIVERSAL=1 -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	MODULE__BLAKE2_CFLAGS = "-I/opt/homebrew/Cellar/libb2/0.98.1/include"
	MODULE__HASHLIB_CFLAGS = "-I/opt/homebrew/opt/openssl@3/include"
	MODULE__LZMA_CFLAGS = "-I/opt/homebrew/Cellar/xz/5.6.3/include"
	MODULE__SSL_CFLAGS = "-I/opt/homebrew/opt/openssl@3/include"
	OPENSSL_INCLUDES = "-I/opt/homebrew/opt/openssl@3/include"
	PY_BUILTIN_MODULE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -DPy_BUILD_CORE_BUILTIN"
	PY_CORE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -DPy_BUILD_CORE"
	PY_CPPFLAGS = "-I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	PY_STDMODULE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
```

```
❯ $(uv python find --python-preference only-managed) -m sysconfig | rg "(INCLUDEPY|include|platinclude)"
	include = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	platinclude = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	CONFIGURE_CPPFLAGS = "-I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	CONFIG_ARGS = "'--prefix=/Users/clm/.pyenv/versions/3.11.11' '--enable-shared' '--libdir=/Users/clm/.pyenv/versions/3.11.11/lib' '--with-openssl=/opt/homebrew/opt/openssl@3' 'PKG_CONFIG_PATH=/opt/homebrew/opt/openssl@3/lib/pkgconfig/:' 'CC=clang' 'LDFLAGS=-L/opt/homebrew/opt/ncurses/lib -L/opt/homebrew/opt/readline/lib -L/opt/homebrew/opt/readline/lib -L/Users/clm/.pyenv/versions/3.11.11/lib -Wl,-rpath,/Users/clm/.pyenv/versions/3.11.11/lib -L/opt/homebrew/lib -Wl,-rpath,/opt/homebrew/lib -L/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib' 'LIBS=-L/Users/clm/.pyenv/versions/3.11.11/lib -Wl,-rpath,/Users/clm/.pyenv/versions/3.11.11/lib -L/opt/homebrew/lib -Wl,-rpath,/opt/homebrew/lib' 'CPPFLAGS=-I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include'"
	CONFINCLUDEDIR = "/Users/clm/.pyenv/versions/3.11.11/include"
	CONFINCLUDEPY = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	CPPFLAGS = "-I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	INCLDIRSTOMAKE = "/Users/clm/.pyenv/versions/3.11.11/include /Users/clm/.pyenv/versions/3.11.11/include /Users/clm/.pyenv/versions/3.11.11/include/python3.11 /Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	INCLUDEDIR = "/Users/clm/.pyenv/versions/3.11.11/include"
	INCLUDEPY = "/Users/clm/.pyenv/versions/3.11.11/include/python3.11"
	LIBEXPAT_CFLAGS = "-I./Modules/expat -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	LIBMPDEC_CFLAGS = "-I./Modules/_decimal/libmpdec -DUNIVERSAL=1 -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	MODULE__BLAKE2_CFLAGS = "-I/opt/homebrew/Cellar/libb2/0.98.1/include"
	MODULE__HASHLIB_CFLAGS = "-I/opt/homebrew/opt/openssl@3/include"
	MODULE__LZMA_CFLAGS = "-I/opt/homebrew/Cellar/xz/5.6.3/include"
	MODULE__SSL_CFLAGS = "-I/opt/homebrew/opt/openssl@3/include"
	OPENSSL_INCLUDES = "-I/opt/homebrew/opt/openssl@3/include"
	PY_BUILTIN_MODULE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -DPy_BUILD_CORE_BUILTIN"
	PY_CORE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -DPy_BUILD_CORE"
	PY_CPPFLAGS = "-I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
	PY_STDMODULE_CFLAGS = "-Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration -fvisibility=hidden  -I./Include/internal -I. -I./Include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/readline/include -I/opt/homebrew/opt/readline/include -I/Users/clm/.pyenv/versions/3.11.11/include -I/opt/homebrew/include -I/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"
```

`diff`ing shows there's no difference. It seems very peculiar that many paths appear to be linked to pyenv-managed interpreter, even with `--python-preference only-managed`

---

_Comment by @neutrinoceros on 2025-01-14 12:52_

I just realized that `uv python find` behaves differently if run from a dir that contains a `.venv` (which is what I did earlier). Apparently `--python-preference` is completely (and silently) ignored in this scenario, which I guess explains why there wasn't any difference between the two outputs.

Running them again from a directory *without* a `.venv`, on the same machine, I get the same output with `--python-preference only-system`, but with `--python-preference only-managed` I now get
```
	include = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13"
	platinclude = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13"
	CONFINCLUDEDIR = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include"
	CONFINCLUDEPY = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13"
	INCLDIRSTOMAKE = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include /Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include /Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 /Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13"
	INCLUDEDIR = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include"
	INCLUDEPY = "/Users/clm/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13"
	LIBHACL_CFLAGS = "-I./Modules/_hacl/include -D_BSD_SOURCE -D_DEFAULT_SOURCE -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/y4/8dcllrc96x32z0w3fgrlx6_m0000gn/T/tmpbyqxzu9h/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new"
```

---

_Comment by @zanieb on 2025-01-14 18:36_

A bit random, but is the problem that there's a space in that include directory path? I've seen that elsewhere. If you delete `/Users/clm/Library/Application Support/uv/` (or move to `~/.local/share/uv`) then we won't use that legacy path.

---

_Comment by @eli-schwartz on 2025-01-14 18:41_

Seeing the log file at /private/tmp/numpy/.mesonpy-xqqxtm9n/meson-logs/meson-log.txt would help to determine whether this is the issue. (I know that pip deletes the log file along with the entire build tree on build failures which is frustrating, but I have no idea what uv might do here.)

---

_Comment by @oscarbenjamin on 2025-01-14 19:35_

> pip deletes the log file along with the entire build tree on build failures

I assume that this works with `uv pip` just like normal `pip`:
```
uv pip install --no-build-isolation
```

---

_Comment by @eli-schwartz on 2025-01-14 19:45_

The logfile is created inside a subdirectory of the extracted sdist, and in at least some cases / versions, for building `.` I seem to recall it produces an sdist or otherwise copies the source tree to a temporary copy. I'm not sure of the exact details in all cases since my preferred frontend is `gpep517 build-wheel` with a config-settings dict passing various meson options including a persistent incremental build directory.

The compilation artifacts need to be available, at any rate.

---

_Comment by @neutrinoceros on 2025-01-15 07:55_

meson's logfile seems to indeed be removed before I get a chance to read it.
Trying `uv build --no-build-isolation` (or `uv pip --no-isolation`), I run into a different issue (which I'm sure is not uv's)

```
❯ uv pip install -e . --no-build-isolation
Resolved 1 package in 3ms
  × Failed to build `numpy @ file:///private/tmp/test-numpy/numpy`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `mesonpy.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
          import mesonpy as backend
      ModuleNotFoundError: No module named 'mesonpy'

      hint: This usually indicates a problem with the package or the build environment.
```
I know that `mesonpy` is shipped as a git submodule within `numpy` (and I do have it checked in), but I don't know how to get `mesonpy` on the path (`uv pip install ./vendored-meson/meson` runs successfully but seems insufficient).

---

_Comment by @neutrinoceros on 2025-01-15 08:07_

> A bit random, but is the problem that there's a space in that include directory path? I've seen that elsewhere. If you delete `/Users/clm/Library/Application Support/uv/` (or move to `~/.local/share/uv`) then we won't use that legacy path.

That's it !
Note that moving the directory wasn't sufficient by itself: I've tried moving the dir but then uv can't find the interpreters it managed, and if I reinstalled them, they landed back at the same location. I also needed to uninstall uv (previously installed via `homebrew`) and reinstall it using the recommended `curl ... | sh` command.

---

_Comment by @oscarbenjamin on 2025-01-15 11:15_

> Trying `uv build --no-build-isolation` (or `uv pip --no-isolation`), I run into a different issue

When you disable build isolation you need to install the dependencies before building e.g. `uv pip install meson-python cython`. I'm not entirely sure how a build without build isolation works with `uv pip` but with `pip` it means that the build steps run in same environment from which `pip` itself is running.

---

_Comment by @neutrinoceros on 2025-01-15 11:43_

I'm aware. But "mesonpy" isn't the name of a PyPI package so I don't know where I'm supposed to get it from.

---

_Comment by @oscarbenjamin on 2025-01-15 11:52_

The package is called meson-python. You can see it in the simpler demo repo I linked above https://github.com/oscarbenjamin/meson_simple/blob/53edd241263528b8551393bbb8a8237b5365880d/pyproject.toml#L1-L3 or in numpy's pyproject.toml: https://github.com/numpy/numpy/blob/bbf4836a82d255c2a2161d277abb4b6f5455ca6d/pyproject.toml#L1-L6

I'm not sure in the case of numpy specifically though because I think numpy at least used to vendor its own fork of meson-python.

For local development with numpy I think it is probably now recommended to use `spin` so the command to make an editable install would be something like `spin install` (first `pip install spin`).

---

_Comment by @neutrinoceros on 2025-01-16 08:22_

Thanks for these pointers !
Since I was able to fix my system by reinstalling uv I don't fancy messing with it again so at the moment I cannot reproduce my own failure. I'm not sure there's a clear actionable left here, so feel free to close @zanieb, and thank you all for your help !

---

_Comment by @zanieb on 2025-01-17 05:18_

Great thanks everyone!

---

_Closed by @zanieb on 2025-01-17 05:18_

---

_Referenced in [astral-sh/uv#11061](../../astral-sh/uv/issues/11061.md) on 2025-01-29 14:14_

---
