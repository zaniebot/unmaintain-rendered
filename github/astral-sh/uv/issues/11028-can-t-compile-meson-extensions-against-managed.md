---
number: 11028
title: "Can't compile Meson extensions against managed Python due to pkg-config"
type: issue
state: closed
author: lgarrison
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-01-28T17:48:40Z
updated_at: 2025-04-01T16:54:21Z
url: https://github.com/astral-sh/uv/issues/11028
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't compile Meson extensions against managed Python due to pkg-config

---

_Issue opened by @lgarrison on 2025-01-28 17:48_

### Summary

When compiling a Python extension using Meson's `py_installation.extension_module()`, Meson looks for the Python development headers using pkg-config. Because the prefix in python-build-standalone's `pc` file is `/install`, the build fails.

The sysconfig paths in PBS are fine, but for better or worse Meson tries pkg-config before sysconfig. There doesn't seem to be a universal way (e.g. one that works for subprojects) to use sysconfig before pkg-config in Meson, but even if there was, it seems like it would be preferable to fix the `pc` file.

Is that possible? Could uv fix the `pc` file after extraction? I guess this wouldn't help non-uv PBS users, however.

I realize this is [documented](https://gregoryszorc.com/docs/python-build-standalone/main/quirks.html#references-to-build-time-paths), so apologies for raising a known issue! But it took a little sleuthing to figure out how Meson was even picking up this `/install` path, so I thought I'd at least share that here.

For completeness, here's the `.pc` file:

<details><summary>python-3.13.pc</summary>
<p>

```
❯ cat /mnt/home/lgarrison/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/pkgconfig/python-3.13.pc
# See: man pkg-config
prefix=/install
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: Python
Description: Build a C extension for Python
Requires:
Version: 3.13
Libs.private: -lpthread -ldl  -lutil
Libs: -L${libdir} 
Cflags: -I${includedir}/python3.13
```

</p>
</details> 

Here's where Meson tries pkg-config before sysconfig: https://github.com/mesonbuild/meson/blob/0b9baf573bf2b1619d7822f67965a35391172625/mesonbuild/dependencies/python.py#L391

Here's the output of Meson's `python_info.py` script that uses sysconfig for discovery (looks fine):

<details>
<summary>python_info.py output</summary>

```
❯ python mesonbuild/scripts/python_info.py | jq | grep /install
    "CONFIG_ARGS": "'--build=x86_64-unknown-linux-gnu' '--host=x86_64-unknown-linux-gnu' '--prefix=/install' '--with-openssl=/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--with-readline=editline' '--enable-shared' '--with-mimalloc' '--enable-optimizations' '--with-lto' '--with-build-python=/tools/host/bin/python3.13' '--with-dbmliborder=bdb' 'build_alias=x86_64-unknown-linux-gnu' 'host_alias=x86_64-unknown-linux-gnu' 'CC=clang' 'CFLAGS= -fPIC ' 'LDFLAGS= -Wl,--exclude-libs,ALL -LModules/_hacl' 'CPPFLAGS= -fPIC '",
    "INSTALL": "/usr/bin/install -c",
    "INSTALL_DATA": "/usr/bin/install -c -m 644",
    "INSTALL_PROGRAM": "/usr/bin/install -c",
    "INSTALL_SCRIPT": "/usr/bin/install -c",
    "INSTALL_SHARED": "/usr/bin/install -c -m 755",
    "WASM_ASSETS_DIR": "./install",
    "WASM_STDLIB": "./install/lib/python3.13/os.py",
```
</details>

And here's a typical compiler command that Meson emits, with `-I/install/...`:

<details>
<summary>Compiler command</summary>

```
ccache cc -I_pycorrfunc.cpython-313-x86_64-linux-gnu.so.p -I. -I../.. -I../../include -I../../lib/theory/DD -I../../lib/common
-I../../subprojects/nanobind-2.2.0/include -I../../subprojects/robin-map-1.3.0/include -I/install/include/python3.13 -fvisibility=hidden
-fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -Wextra -O3 -DPYCORRFUNC_USE_DOUBLEACCUM -fPIC -fopenmp -ffunction-sections
-fdata-sections -DPYCORRFUNC_USE_DOUBLE -DNB_DOMAIN=d -funroll-loops -MD -MQ _pycorrfunc.cpython-313-x86_64-linux-gnu.so.p/lib_theory_DD_countpairs.c.o
-MF _pycorrfunc.cpython-313-x86_64-linux-gnu.so.p/lib_theory_DD_countpairs.c.o.d -o
_pycorrfunc.cpython-313-x86_64-linux-gnu.so.p/lib_theory_DD_countpairs.c.o -c ../../lib/theory/DD/countpairs.c
../../lib/theory/DD/countpairs.c:7:10: fatal error: Python.h: No such file or directory
 #include <Python.h>
          ^~~~~~~~~~
```
</details>

There's a constellation of sort-of similar uv issues around sysconfig variables (e.g. those linked from https://github.com/astral-sh/uv/issues/8429), but I think in this case the issue is specifically with the pkg-config file. I couldn't find an existing issue about that.

## Steps to reproduce
Here is a reproducer on a small project. I'm happy to work on making it more minimal if that would be helpful.

```
❯ git clone https://github.com/lgarrison/pycorrfunc.git
❯ cd pycorrfunc
❯ git checkout c869af8feb936960ef3673aa0974eb332c47fc8a
❯ uv sync --python-preference=only-managed
```


### Platform

Rocky 8 Linux

### Version

0.5.24

### Python version

Python 3.13

---

_Label `bug` added by @lgarrison on 2025-01-28 17:48_

---

_Comment by @zanieb on 2025-01-28 17:59_

cc @eli-schwartz since you've been active on meson-related issues; do you have context on the motivation for this behavior?

I'm fine with patching the pkg-config files on install, as we do for the macOS dylib (#10629). Are you interested in contributing a fix?

---

_Assigned to @zanieb by @zanieb on 2025-01-28 17:59_

---

_Label `great writeup` added by @zanieb on 2025-01-28 17:59_

---

_Comment by @lgarrison on 2025-01-28 18:09_

Happy to give it a shot. Time to put those Advent of Code Rust skills to use!

---

_Comment by @eli-schwartz on 2025-01-28 18:25_

Meson needs to know the "official interface flags" for linking to libpython, primarily:
- what `-I/path/to/Python-dot-h` path to use for compiling
- what `-L/path/to/libpython -lpython3.XX` flags to use for linking:
  - when `embed: true` is used, for example when compiling a python loadable plugin for a C++ application that includes scripting support for perl/python/guile/lua/ruby/tcl
  - on platforms such as Android or Cygwin, where linking to libpython is required even for compiling importable modules

Doing this accurately is a bit of a hodgepodge. The base code for implementing this is at https://github.com/mesonbuild/meson/blob/master/mesonbuild/dependencies/python.py

Notice there are two methods: pkg-config, and sysconfig. The sysconfig method uses `find_libpy()` and `find_libpy_windows()` which you will note doesn't actually use sysconfig directly, but does various searches by manually constructing details from sysconfig `base`, `DEBUG_EXT`, `ABIFLAGS`, `py_version_nodot`, `py_version_short`, `implementation_lower`, `ABI3DLLLIBRARY`, `INCLUDEPY`, `include`, `platinclude`, `ABI3DLLLIBRARY`, `ABI3LDLIBRARY`, `LDLIBRARY`, `LIBDIR`, etc, as well as hardcoding certain known assumptions about the shape of the install layout for PyPy.

This will hopefully get a lot better when https://peps.python.org/pep-0739/ is approved and deployed, which guarantees that any platform can describe via JSON what pkg-config already consistently describes (but requires pkg-config to be installed, which is not necessarily the case on Windows).

> Here's the output of Meson's `python_info.py` script that uses sysconfig for discovery (looks fine):

python_info.py is used for discovery of various things pkg-config doesn't describe, like "whether you are in a venv" or "what are the standard paths for site-packages etc", "am I PyPY or CPython", "am I free-threaded", "what is the limited API suffix", etc. We also save the exhaustive details of *every* sysconfig variable, just in case we need it later on (and indeed it's needed for the guesswork in `dependencies/python.py`).

---

_Comment by @zanieb on 2025-01-28 18:35_

If the target from `pkg-config` does not exist should it fallback to `sysconfig`?

---

_Comment by @eli-schwartz on 2025-01-28 18:57_

Currently it assumes the flags are trusted and passes them to the compiler -- it's the compiler that decides they aren't trusted. There are some nuances around for example `-I/usr/include/python3.13` where that path doesn't exist on your machine, but the compiler will locate it via a sysroot in `/usr/aarch64-pc-linux-gnu/usr/include/python3.13` so it's not immediately obvious when to second-guess the results.

---

_Comment by @eli-schwartz on 2025-01-28 19:19_

> cc [@eli-schwartz](https://github.com/eli-schwartz) since you've been active on meson-related issues

Aside: thanks for pinging me on this. :heart: I'm interested in helping people with meson issues and this overlaps with my interest in the python ecosystem / building code, so I'm absolutely comfortable with being pinged any time you have a question for me or simply think I'd be interested. But I only irregularly remember to check issue trackers, so it's best to summon me explicitly to ensure I notice.

---

_Comment by @geofft on 2025-01-28 23:23_

Would it help anything to make the .pc file use relative paths from `${pcfiledir}`? From `man pkg-config`:

> The installed location of the .pc file. This can be used to query the location of the .pc file for a particular module, but it can also be used to make .pc files relocatable. For instance:
> ```
> prefix=${pcfiledir}/../..
> exec_prefix=${prefix}
> libdir=${exec_prefix}/lib
> includedir=${prefix}/include
> ```

This appears to be what Meson's own `pkgconfig.relocatable` option does for .pc files it produces. (See also a little discussion in openssl/openssl#13080 which points out some issue with sysroots which might not be relevant to us.)

If so then we can just make this change in python-build-standalone and avoid having to patch on install.

---

_Comment by @zanieb on 2025-01-28 23:32_

Oh cool, that definitely seems like a better solution if it's viable.

---

_Comment by @eli-schwartz on 2025-01-28 23:35_

relocatable pkg-config files are a ***really bad*** tradeoff for a project that expects to be installed to a non-relocatable place, i.e. /usr and simply have done with. You never ever use the functionality, so any cost is too much. Furthermore, it results in paths such as `-I/usr/lib64/pkgconfig/../../include` which aren't great since that's a compiler default search path and normally, without `--keep-system-cflags`, those get stripped.

 So of course, openssl cannot simply use them and have done with, which was what that ticket was about.

For python-build-standalone, the same tradeoffs can be made that meson makes when you explicitly enable relocation. You know, on a per project or per binary distribution basis, that it is *very* likely to be relocated, thus by default the flags are simply broken, but relocatable pkgconfig files transition it from being "broken all the time" to "working most of the time". That makes it very much worthwhile.

Additionally, uv may not be concerned about people installing python using uv in order to cross compile with a sysroot. Probably, most people using sysroots have careful setups which include building cpython from source. Also, pkgconf is pretty popular these days and people generally don't use the original pkg-config (it isn't 2020 anymore!)

So all in all I think you should use relocatable pkg-config files, yes.


---

_Comment by @geofft on 2025-01-29 01:04_

Wait, I just saw we already patch the pkg-config files after extracting in #10189, which should be in the version of uv you're using - why isn't that helping here? `~/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/pkgconfig/python3.pc` does seem to be patched on my system now that I look at it.

(I was looking at a plain python-build-standalone tarball.)

Does doing a `uv python uninstall 3.13 && uv python install 3.13` help?

---

_Comment by @lgarrison on 2025-01-29 01:31_

... yes it does. Sorry for not catching that!

There may still be value in making the pkg-config file relocatable upstream; it would make the `pc` file more useful for non-uv users, and it would save uv from having to patch it at runtime. What do you think?

---

_Comment by @geofft on 2025-01-29 01:41_

Yes, absolutely - I kind of want to get to the point where uv is no longer patching anything from python-build-standalone.

---

_Comment by @samypr100 on 2025-01-29 03:09_

If there's still interest in patching, https://github.com/bluss/sysconfigpatcher does already patch .pc files correctly from my past usage experience and it can serve as inspiration (or for testing).

---

_Comment by @charliermarsh on 2025-01-29 03:21_

(@geofft -- Just confirming that, yes, we do already patch `.pc` files -- or, at least, we _should_ be patching `.pc` files.)

---

_Comment by @geofft on 2025-01-29 03:29_

@samypr100 https://github.com/astral-sh/uv/blob/0.5.25/crates/uv-python/src/sysconfig/mod.rs does actually reference that project in the comments at the top, so I think that's approximately what we're currently doing. Thanks for the pointer, though, in the sense that it would be nice to make sure that, eventually, neither sysconfigpatcher nor the uv port is necessary :)

---

_Comment by @samypr100 on 2025-01-29 04:09_

My bad @charliermarsh, I completely missed .pc file patching was implemented a bit later after the initial round of patching PR's😆 

(I thought it was still a TODO thing)

---

_Referenced in [astral-sh/uv#11234](../../astral-sh/uv/issues/11234.md) on 2025-02-05 06:45_

---

_Referenced in [astral-sh/uv#11291](../../astral-sh/uv/pulls/11291.md) on 2025-02-06 19:49_

---

_Referenced in [astropy/astropy#17760](../../astropy/astropy/issues/17760.md) on 2025-02-13 07:29_

---

_Comment by @zanieb on 2025-04-01 16:54_

I think this is fixed?

---

_Closed by @zanieb on 2025-04-01 16:54_

---
