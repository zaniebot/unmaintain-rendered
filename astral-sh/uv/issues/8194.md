```yaml
number: 8194
title: "`uv python install 3.13t` issues on Windows and Linux"
type: issue
state: closed
author: colesbury
labels:
  - bug
assignees: []
created_at: 2024-10-15T03:15:32Z
updated_at: 2024-10-18T05:44:56Z
url: https://github.com/astral-sh/uv/issues/8194
synced_at: 2026-01-10T04:45:10Z
```

# `uv python install 3.13t` issues on Windows and Linux

---

_Issue opened by @colesbury on 2024-10-15 03:15_

Nice work! This is my favorite way to install Python. I ran into two issues:

On Windows, `uv python install 3.13t` doesn't seem to install a free-threaded build. 

```shell
> uv python install 3.13t
> uv python list
cpython-3.13.0-windows-x86_64-none     AppData\Roaming\uv\python\cpython-3.13.0+freethreaded-windows-x86_64-none\python.exe

> uv run -p 3.13t python -c "import sysconfig; print(sysconfig.get_config_var('Py_GIL_DISABLED'))"
0
```

On Linux,  `uv python install 3.13t`  installs a debug build:

```shell
> uv run -p 3.13t python -c "import sysconfig; print(sysconfig.get_config_var('CFLAGS'))"
-fno-strict-overflow -Wsign-compare -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -Og -Wall  -fPIC
```

Whereas `3.13` (GIL) is an optimized build, like I'd expect:

```shell
> uv run -p 3.13 python -c "import sysconfig; print(sysconfig.get_config_var('CFLAGS'))"
-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC
```

---

_Comment by @zanieb on 2024-10-15 03:19_

Thanks for the report! I'll look into this.

---

_Label `bug` added by @zanieb on 2024-10-15 03:19_

---

_Assigned to @zanieb by @zanieb on 2024-10-15 03:19_

---

_Comment by @colesbury on 2024-10-15 15:42_

In case this is helpful, for the free threaded Windows build:

* If you use the batch scripts (e.g., `PCBuild/build.bat`), you should pass `--disable-gil`
* If you use MSBuild (on `PCBuild/pcbuild.proj`), you should pass `/p:DisableGil=true`
* IIRC, MSBuild copies `PC/pyconfig.h.in` to `PC/pyconfig.h` and [replaces](https://github.com/python/cpython/blob/b903fc38d8ec74f61fb5ca8a3f5fd025915bceac/PCbuild/pythoncore.vcxproj#L703) the `/* #define Py_GIL_DISABLED 1 */` with `#define Py_GIL_DISABLED 1`

---

_Comment by @colesbury on 2024-10-15 15:45_

Oh, I see now you already have a PR in progress: https://github.com/indygreg/python-build-standalone/pull/368.

---

_Comment by @zanieb on 2024-10-15 17:11_

Yep! Sorry about that. I should have the fix out soon — just takes a while to build all the distributions over there.

---

_Comment by @charliermarsh on 2024-10-17 01:34_

@zanieb -- Is this one closed out given your recent merge?

---

_Comment by @zanieb on 2024-10-17 01:38_

Yep! 

Closed by https://github.com/astral-sh/uv/pull/8196 and https://github.com/astral-sh/uv/pull/8268

Will be released soon.

---

_Closed by @zanieb on 2024-10-17 01:38_

---

_Comment by @farzbood on 2024-10-18 05:41_

based on [Python 3.13.0 docs](https://docs.python.org/3/whatsnew/3.13.html#free-threaded-cpython)

> To check if the current interpreter supports free-threading, [python -VV](https://docs.python.org/3/using/cmdline.html#cmdoption-V) and [sys.version](https://docs.python.org/3/library/sys.html#sys.version) contain “experimental free-threading build”.
but the managed `python 3.13t` installed by `uv python install 3.13t` doesn't show the expected output!
![image](https://github.com/user-attachments/assets/9510f8c0-4f96-46aa-907b-1e49e79ce0b2)
(edit):
how to remove/unistall a managed python version? deleting the installation path take care of it?

---
