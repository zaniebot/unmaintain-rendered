---
number: 9580
title: "[macOS] ModuleNotFoundError: No module named 'encodings' when compiling with meson"
type: issue
state: closed
author: ben-xD
labels:
  - question
assignees: []
created_at: 2024-12-02T18:43:45Z
updated_at: 2025-04-29T14:05:43Z
url: https://github.com/astral-sh/uv/issues/9580
synced_at: 2026-01-07T13:12:18-06:00
---

# [macOS] ModuleNotFoundError: No module named 'encodings' when compiling with meson

---

_Issue opened by @ben-xD on 2024-12-02 18:43_

I'm trying to build gstreamer locally with python 3.11 from uv. I've managed to build gstreamer locally with the **homebrew** python. I use python 3.11 because the gstreamer version im using has a dependency that relies on distutils, removed in python3.12.

I would like to use python by uv. I modified the steps by @samypr100 in https://github.com/astral-sh/uv/issues/8966#issuecomment-2466513707

## Reproducible steps
```bash
# Some dependencies of packages
brew install pkg-config ninja

git clone --branch 1.24 https://gitlab.freedesktop.org/gstreamer/gstreamer.git
pushd gstreamer

uv python install 3.11
uv venv -p 3.11
uv tool install 'git+https://github.com/bluss/sysconfigpatcher'
sysconfigpatcher $HOME/.local/share/uv/python/cpython-3.11*
uv pip install meson docutils

# Export PYTHON so the gstreamer build will use it
export PYTHON="$HOME:$(pwd)/.venv/bin/python"

# Build and install gstreamer
PKG_CONFIG_LIBDIR=/dev/null uv run meson setup build -Dwrap_mode=forcefallback -Dintrospection=enabled -Dpython=enabled -Dgst-plugins-good:lame=disabled -Dglib:tests=false -Dtests=disabled --prefix=$HOME/gstreamer

## FAILS HERE
uv run meson compile -C build

# Install to $HOME/gstreamer
uv run meson install -C build
```

## Error

```
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'
```

Would you have any advice on this error?

## Full error

<details>
  <summary>Click to expand</summary>

```
uv run meson compile -C build
INFO: autodetecting backend as ninja
INFO: calculating backend command to run: /opt/homebrew/bin/ninja -C /Users/user/repos/gstreamer/build
ninja: Entering directory `/Users/user/repos/gstreamer/build'
[2/3] Generating subprojects/gst-editing-services/ges/GES-1.0.gir with a custom command (wrapped by meson to set env)
FAILED: subprojects/gst-editing-services/ges/GES-1.0.gir
env PKG_CONFIG_PATH=/Users/user/repos/gstreamer/build/meson-uninstalled PKG_CONFIG=/opt/homebrew/bin/pkg-config CC=cc /Users/user/repos/gstreamer/build/subprojects/gobject-introspection-1.74.0/tools/g-ir-scanner --quiet --no-libtool --namespace=GES --nsversion=1.0 --warn-all --output subprojects/gst-editing-services/ges/GES-1.0.gir '--add-init-section=extern void gst_init(gint*,gchar**);extern void ges_init(void);g_setenv("GST_REGISTRY_1.0", "/no/way/this/exists.reg", TRUE);g_setenv("GST_PLUGIN_PATH_1_0", "", TRUE);g_setenv("GST_PLUGIN_SYSTEM_PATH_1_0", "", TRUE);g_setenv("GST_DEBUG", "0", TRUE);gst_init(NULL,NULL);ges_init();' --quiet --c-include=ges/ges.h --cflags-begin -I/Users/user/repos/gstreamer/subprojects/gst-editing-services/ges/.. -I/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/ges/.. --cflags-end -I/Users/user/repos/gstreamer/subprojects/gst-editing-services/ges -I/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/ges -I/Users/user/repos/gstreamer/subprojects/gst-editing-services/. -I/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/. -I/Users/user/repos/gstreamer/subprojects/gstreamer/. -I/Users/user/repos/gstreamer/build/subprojects/gstreamer/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/. -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/glib -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/glib -I/Users/user/repos/gstreamer/subprojects/proxy-libintl/. -I/Users/user/repos/gstreamer/build/subprojects/proxy-libintl/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gobject -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gobject -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gmodule -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gmodule -I/Users/user/repos/gstreamer/subprojects/gstreamer/libs -I/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs -I/Users/user/repos/gstreamer/subprojects/gst-plugins-base/gst-libs -I/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs -I/Users/user/repos/gstreamer/subprojects/orc/. -I/Users/user/repos/gstreamer/build/subprojects/orc/. -I/Users/user/repos/gstreamer/subprojects/zlib-1.2.13/. -I/Users/user/repos/gstreamer/build/subprojects/zlib-1.2.13/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gio -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gio -I/Users/user/repos/gstreamer/subprojects/libxml2-2.10.3/include -I/Users/user/repos/gstreamer/build/subprojects/libxml2-2.10.3/include -I/Users/user/repos/gstreamer/subprojects/gst-devtools/validate/. -I/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/. -I/Users/user/repos/gstreamer/subprojects/json-glib-1.6.6/. -I/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/. --filelist=/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/ges/libges-1.0.0.dylib.p/GES_1.0_gir_filelist --include=Gst-1.0 --include=GstPbutils-1.0 --include=GstVideo-1.0 --include=Gio-2.0 --include=GObject-2.0 --symbol-prefix=ges --identifier-prefix=GES --pkg-export=gst-editing-services-1.0 --cflags-begin -I/Users/user/repos/gstreamer/subprojects/gst-editing-services/. -I/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/. -I/Users/user/repos/gstreamer/subprojects/gstreamer/. -I/Users/user/repos/gstreamer/build/subprojects/gstreamer/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/. -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/glib -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/glib -I/Users/user/repos/gstreamer/subprojects/proxy-libintl/. -I/Users/user/repos/gstreamer/build/subprojects/proxy-libintl/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gobject -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gobject -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gmodule -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gmodule -I/Users/user/repos/gstreamer/subprojects/gstreamer/libs -I/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs -I/Users/user/repos/gstreamer/subprojects/gst-plugins-base/gst-libs -I/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs -I/Users/user/repos/gstreamer/subprojects/orc/. -I/Users/user/repos/gstreamer/build/subprojects/orc/. -I/Users/user/repos/gstreamer/subprojects/zlib-1.2.13/. -I/Users/user/repos/gstreamer/build/subprojects/zlib-1.2.13/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/gio -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gio -I/Users/user/repos/gstreamer/subprojects/libxml2-2.10.3/include -I/Users/user/repos/gstreamer/build/subprojects/libxml2-2.10.3/include -I/Users/user/repos/gstreamer/subprojects/gst-devtools/validate/. -I/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/. -I/Users/user/repos/gstreamer/subprojects/json-glib-1.6.6/. -I/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/. -I/Users/user/repos/gstreamer/subprojects/gstreamer/gst/parse -I/Users/user/repos/gstreamer/build/subprojects/gstreamer/gst/parse -I/Users/user/repos/gstreamer/subprojects/pcre2-10.42/. -I/Users/user/repos/gstreamer/build/subprojects/pcre2-10.42/. -I/Users/user/repos/gstreamer/subprojects/pcre2-10.42/src -I/Users/user/repos/gstreamer/build/subprojects/pcre2-10.42/src -I/Users/user/repos/gstreamer/subprojects/libffi/. -I/Users/user/repos/gstreamer/build/subprojects/libffi/. -I/Users/user/repos/gstreamer/subprojects/libffi/include -I/Users/user/repos/gstreamer/build/subprojects/libffi/include -I/Users/user/repos/gstreamer/subprojects/gst-plugins-base/. -I/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/. -I/Users/user/repos/gstreamer/subprojects/glib-2.78.3/subprojects/gvdb/. -I/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/subprojects/gvdb/. -I/opt/homebrew/include -DPCRE2_STATIC -I/Users/user/repos/gstreamer/subprojects/gobject-introspection-1.74.0/girepository/. -I/Users/user/repos/gstreamer/build/subprojects/gobject-introspection-1.74.0/girepository/. -I/opt/homebrew/opt/libffi/include --cflags-end --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gstreamer/gst --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/base --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/video --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/tag --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/audio --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/pbutils --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/controller --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/check --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/app --add-include-path=/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/json-glib --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/gst/validate --add-include-path=/Users/user/repos/gstreamer/build/subprojects/gobject-introspection-1.74.0/gir -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/gst -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/glib -L/Users/user/repos/gstreamer/build/subprojects/proxy-libintl -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gobject -L/Users/user/repos/gstreamer/build/subprojects/libffi/src -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gmodule --extra-library=gstreamer-1.0 --extra-library=glib-2.0 --extra-library=intl --extra-library=gobject-2.0 --extra-library=gmodule-2.0 -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/base --extra-library=gstbase-1.0 -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/video -L/Users/user/repos/gstreamer/build/subprojects/orc/orc --extra-library=gstvideo-1.0 --extra-library=orc-0.4 -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/pbutils -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/audio -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/tag -L/Users/user/repos/gstreamer/build/subprojects/zlib-1.2.13 --extra-library=gstpbutils-1.0 --extra-library=gstaudio-1.0 --extra-library=gsttag-1.0 --extra-library=z -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/controller --extra-library=gstcontroller-1.0 -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gio --extra-library=gio-2.0 -L/Users/user/repos/gstreamer/build/subprojects/libxml2-2.10.3 --extra-library=xml2 -L/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/gst/validate -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/check -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/app -L/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/json-glib --extra-library=gstvalidate-1.0 --extra-library=gstcheck-1.0 --extra-library=gstapp-1.0 --extra-library=json-glib-1.0 -L/Users/user/repos/gstreamer/build/subprojects/gobject-introspection-1.74.0/girepository --extra-library=girepository-1.0 -L/Users/user/repos/gstreamer/build/subprojects/gst-editing-services/ges --library ges-1.0 -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/gst -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/glib -L/Users/user/repos/gstreamer/build/subprojects/proxy-libintl -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gobject -L/Users/user/repos/gstreamer/build/subprojects/libffi/src -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gmodule -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/base -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/video -L/Users/user/repos/gstreamer/build/subprojects/orc/orc -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/pbutils -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/audio -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/tag -L/Users/user/repos/gstreamer/build/subprojects/zlib-1.2.13 -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/controller -L/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gio -L/Users/user/repos/gstreamer/build/subprojects/libxml2-2.10.3 -L/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/gst/validate -L/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/check -L/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/app -L/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/json-glib -L/Users/user/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/lib -L/opt/homebrew/opt/libffi/lib --extra-library=m --extra-library=iconv --extra-library=readline -L/Users/user/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/lib --extra-library=python3.11 --extra-library=dl --extra-library=resolv --sources-top-dirs /Users/user/repos/gstreamer/subprojects/gst-editing-services --sources-top-dirs /Users/user/repos/gstreamer/build/subprojects/gst-editing-services
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gstreamer/gst' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/glib' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/proxy-libintl' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gobject' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/libffi/src' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gmodule' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/base' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/video' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/orc/orc' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/pbutils' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/audio' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/tag' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/zlib-1.2.13' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/controller' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/glib-2.78.3/gio' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/libxml2-2.10.3' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-devtools/validate/gst/validate' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gstreamer/libs/gst/check' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/gst-plugins-base/gst-libs/gst/app' ignored
ld: warning: duplicate -rpath '/Users/user/repos/gstreamer/build/subprojects/json-glib-1.6.6/json-glib' ignored
ld: warning: duplicate -rpath '/Users/user/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/lib' ignored
ld: warning: ignoring duplicate libraries: '-lgio-2.0', '-lglib-2.0', '-lgmodule-2.0', '-lgobject-2.0', '-lintl'
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = 'python3'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.11'
  sys._base_executable = '/Users/user/repos/gstreamer/build/tmp-introspectk7kz6uwj/GES-1.0'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/Users/user/repos/gstreamer/build/tmp-introspectk7kz6uwj/GES-1.0'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python311.zip',
    '/install/lib/python3.11',
    '/install/lib/python3.11/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x00000001ff8f7840 (most recent call first):
  <no Python frame>
Command '['/Users/user/repos/gstreamer/build/tmp-introspectk7kz6uwj/GES-1.0', '--introspect-dump=/Users/user/repos/gstreamer/build/tmp-introspectk7kz6uwj/functions.txt,/Users/user/repos/gstreamer/build/tmp-introspectk7kz6uwj/dump.xml']' returned non-zero exit status 1.
ninja: build stopped: subcommand failed.
```
</details>

---

_Comment by @samypr100 on 2024-12-03 02:05_

I tried to repro, but hit a bump on meson setup with `subprojects/gobject-introspection-1.74.0/meson.build:129:11: ERROR: Recursive include of subprojects: gstreamer => glib => gobject-introspection => glib.`

My initial thoughts is that it's possible this is related to meson issues discussed as part of https://github.com/astral-sh/uv/pull/7857 as meson doesn't work with build isolation. From a python meson setup I also expected to see `/Users/user/repos/gstreamer/.venv` or `'$HOME/.local/share/uv/python/cpython-3.11*` instead of `/install`.

Do you have a docker based reproduceable steps?

---

_Label `question` added by @samypr100 on 2024-12-03 02:06_

---

_Comment by @ben-xD on 2024-12-03 10:27_

Apologies @samypr100, there are many things that can go wrong with the gstreamer stuff. Gstreamer currently has a broken `main` branch lol where there is a hard circular dependency... Please try on branch `1.24`, that works for me. I've also updated my first messsage
```git
git checkout 1.24
```

A docker setup wouldn't let me use brew, native macOS etc so I haven't tried to set that up.

About build isolation, setting `-Dwrap_mode=forcefallback` will not use any of the system libraries, and download everything. That allows the build to be a bit more isolated, but I do need to install some things (currently python including headers from brew, ninja, meson).

---

_Comment by @samypr100 on 2024-12-04 05:15_

Hmm, still not happy. Same error.

Re: build isolation, I meant Python PEP 517 build isolation, not meson's build isolation with respect to system libraries 😅 

---

_Comment by @ben-xD on 2024-12-04 09:11_

Sorry @samypr100, I believe you need to reset the subprojects and build folder. Try `git clean -fXd build subprojects`. You'll need to automatically download the dependencies again, but this should work.

---

_Comment by @samypr100 on 2024-12-08 05:27_

Could you maybe try building in a clean environment, e.g. via a `ghcr.io/astral-sh/uv:debian` docker container?

That's what I've been using (always pristine). I was able to get 1.24 branch to build, compile, and install after removing `-Dwrap_mode=forcefallback` to avoid the `Recursive include` issue. My patched uv python also had the right pkgconfig settings

```
# See: man pkg-config
prefix=/root/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: Python
Description: Build a C extension for Python
Requires:
Version: 3.11
Libs.private: -lpthread -ldl  -lutil
Libs:
Cflags: -I${includedir}/python3.11
```

---

_Comment by @ben-xD on 2024-12-08 08:42_

I'm trying to build the macOS library rather than the linux one so I can load it on macOS without docker. Using a `debian` based docker image wouldn't be useful for me even if it works? 

The recursive include shouldn't affect 1.24 - that might've happened because you didn't clean your macOS build (subprojects folder).

Removing forcefallback means you might be using system libraries instead. According to the gstreamer devs, force fallback is needed to enable gobject-introspection (something I need), so that's why I'm using forcefallback too 😅 

---

_Comment by @samypr100 on 2024-12-08 14:59_

Does it work for you on `debian` just not on macOS?

---

_Comment by @zanieb on 2025-01-07 19:34_

Closing as stale, feel free to follow-up if you are having problems still.

---

_Closed by @zanieb on 2025-01-07 19:34_

---

_Comment by @tkukushkin on 2025-01-20 20:24_

Hello! Still have similar problem:

```
❯ ls -lah /Users/tkukushkin/.local/bin/python3.13
... /Users/tkukushkin/.local/bin/python3.13 -> /Users/tkukushkin/Library/Application Support/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13

❯ /Users/tkukushkin/.local/bin/python3.13 -m venv venv
Error: Command '['/Users/tkukushkin/venv/bin/python3.13', '-m', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.

❯ /Users/tkukushkin/venv/bin/python3.13 -m ensurepip --upgrade --default-pip
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Fatal Python error: Failed to import encodings module
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x0000000201107840 (most recent call first):
  <no Python frame>
```

It's not really relevant for me, I just always create virtual environments directly with uv. And I haven't seen such errors in any other situations.




---

_Comment by @zanieb on 2025-01-20 21:28_

If we're to debug this further, I think we'll need an isolated, minimal reproduction in a Dockerfile.

@tkukushkin did you ensure that you're on the latest uv-managed Python version? `uv self update && uv python install 3.13 --reinstall` may be needed — we've added patches to help with module discovery in extension modules.

---

_Comment by @tkukushkin on 2025-01-20 21:37_

@zanieb It doesn't help.


---

_Comment by @samypr100 on 2025-01-20 21:42_

@tkukushkin It might help if you can open a new separate issue following the steps from https://github.com/astral-sh/uv/issues/9452

---

_Comment by @zanieb on 2025-01-20 21:54_

Yeah we're happy to help, we just need a clear way to reproduce the problem so we can debug it. A container is best, since it's isolated from your system state.

---

_Comment by @tkukushkin on 2025-01-21 04:21_

I fully agree, but I am not sure if this problem is relevant to linux and I am not sure that Darwin image will be helpful.

---

_Comment by @ahitrin on 2025-04-29 13:50_

FYI: I've got a similar issue when running `uv` inside a Linux container when host system is MacOS X.

**Host system**: MacOS X 14.7 Sonoma (M3 Pro)

**Docker**: OrbStack 1.10.3

**Container**: `debian:latest`

Steps to reproduce:

```shell
# In a host shell
docker run -ti --rm debian:latest

# Everything below is executed inside a container
apt update -y && apt install -y curl
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH=/root/.local/bin:$PATH
uv python install 3.12 --preview
python3.12 -m venv /root/venv
```

Got error on the last step:

```shell
Error: Command '['/root/venv/bin/python3.12', '-m', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.
```

When running this command within the same container shell:
```shell
# /root/venv/bin/python3.12 -m ensurepip --upgrade --default-pip
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/root/venv/bin/python3.12'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/root/.local/share/uv/python/cpython-3.12.10-linux-aarch64-gnu/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/root/venv/bin/python3.12'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x0000ffff87beb020 (most recent call first):
  <no Python frame>
```

That's interesting: module `encodings` is available in the "global" python but not from venv:
```shell
# /root/venv/bin/python3.12 -c 'import encodings'
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/root/venv/bin/python3.12'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/root/.local/share/uv/python/cpython-3.12.10-linux-aarch64-gnu/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/root/venv/bin/python3.12'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x0000ffff86451020 (most recent call first):
  <no Python frame>

# python3.12 -c 'import encodings'           # no error
```

---

_Comment by @konstin on 2025-04-29 14:05_

That is another case of https://github.com/astral-sh/python-build-standalone/issues/380, though the preview shims make it easier since the shim is a symlink. fwiw, `uv venv` will create a working venv as it knows about the PYTHONHOME problem and has a workaround for it.

---
