---
number: 1238
title: "Puffin can't build ujson on windows due to pip difference"
type: issue
state: closed
author: konstin
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-02T19:55:05Z
updated_at: 2024-02-12T00:51:38Z
url: https://github.com/astral-sh/uv/issues/1238
synced_at: 2026-01-10T01:23:05Z
---

# Puffin can't build ujson on windows due to pip difference

---

_Issue opened by @konstin on 2024-02-02 19:55_

 The `ujson` package includes a `[build-system]`, but no `build-backend`. It installs with pip but fails with puffin.

Specifically,
```
pip install https://files.pythonhosted.org/packages/43/1a/b0a027144aa5c8f4ea654f4afdd634578b450807bb70b9f8bad00d6f6d3c/ujson-5.7.0.tar.gz
```
works, while
```
cargo run -- pip install "ujson @ https://files.pythonhosted.org/packages/43/1a/b0a027144aa5c8f4ea654f4afdd634578b450807bb70b9f8bad00d6f6d3c/ujson-5.7.0.tar.gz" 
```
does not:
```
Resolved 1 package in 2.21s
error: Failed to download distributions
  Caused by: Failed to fetch wheel: ujson @ https://files.pythonhosted.org/packages/43/1a/b0a027144aa5c8f4ea654f4afdd634578b450807bb70b9f8bad00d6f6d3c/ujson-5.7.0.tar.gz
  Caused by: Failed to build: ujson @ https://files.pythonhosted.org/packages/43/1a/b0a027144aa5c8f4ea654f4afdd634578b450807bb70b9f8bad00d6f6d3c/ujson-5.7.0.tar.gz
  Caused by: Build backend failed to build wheel through `build_wheel()`:
--- stdout:
running bdist_wheel
running build
running build_ext
building 'ujson' extension
creating build
creating build\temp.win-amd64-cpython-312
creating build\temp.win-amd64-cpython-312\Release
creating build\temp.win-amd64-cpython-312\Release\deps
creating build\temp.win-amd64-cpython-312\Release\deps\double-conversion
creating build\temp.win-amd64-cpython-312\Release\deps\double-conversion\double-conversion
creating build\temp.win-amd64-cpython-312\Release\lib
creating build\temp.win-amd64-cpython-312\Release\python
"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.38.33130\bin\HostX86\x64\cl.exe" /c /nologo /O2 /W3 /
GL /DNDEBUG /MD -I./python -I./lib -I./deps/double-conversion/double-conversion -IC:\Users\Konstantin\AppData\Local\Temp\.tmp4B
tsR8\.venv\include -IC:\Users\Konstantin\AppData\Local\Programs\Python\Python312\include -IC:\Users\Konstantin\AppData\Local\Pr
ograms\Python\Python312\Include "-IC:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.38.33130\include" "
-IC:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IC:\Program Files (x86)\Windows Kits\10\in
clude\10.0.22621.0\ucrt" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\um" "-IC:\Program Files (x86)\Windows
 Kits\10\\include\10.0.22621.0\\shared" "-IC:\Program Files (x86)\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IC:\Program F
iles (x86)\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" /EHsc /Tp./deps/double-conversion/double-conversion\bignum-dtoa.cc /Fobuild\temp.win-amd64-cpython-312\Release\./deps/double-conversion/double-conversion\bignum-dtoa.obj -D_GNU_SOURCE
bignum-dtoa.cc
c1xx: fatal error C1083: Datei (Quelle) kann nicht ge�ffnet werden: "./deps/double-conversion/double-conversion\bignum-dtoa.cc": No such file or directory
--- stderr:
WARNING setuptools_scm.pyproject_reading toml section missing 'pyproject.toml does not contain a tool.setuptools_scm section'  
error: command 'C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\VC\\Tools\\MSVC\\14.38.33130\\bin\\HostX86\\x64\\cl.exe' failed with exit code 2
---
error: process didn't exit successfully: `target\debug\puffin.exe pip install "ujson @ https://files.pythonhosted.org/packages/43/1a/b0a027144aa5c8f4ea654f4afdd634578b450807bb70b9f8bad00d6f6d3c/ujson-5.7.0.tar.gz"` (exit code: 2)
```

We need to figure out what different setuptools incantation pip performs and replicate it. 

---

_Label `bug` added by @konstin on 2024-02-02 19:55_

---

_Label `windows` added by @konstin on 2024-02-02 19:55_

---

_Comment by @charliermarsh on 2024-02-02 19:56_

Does `pypa/build` build it?

---

_Comment by @konstin on 2024-02-02 19:59_

Yes that works too

---

_Added to milestone `Initial release` by @konstin on 2024-02-02 20:00_

---

_Comment by @charliermarsh on 2024-02-02 20:01_

🤷‍♂️ We do basically the exact same thing.

---

_Comment by @konstin on 2024-02-02 20:02_

Even if `build-backend` is not specified while `[build-system]` is? That is the test case this is coming from

---

_Comment by @charliermarsh on 2024-02-02 20:02_

Can try with `--legacy-setup-py`.

---

_Comment by @charliermarsh on 2024-02-02 20:03_

Yes, I believe so: https://github.com/astral-sh/puffin/pull/530

---

_Comment by @charliermarsh on 2024-02-02 20:04_

`ujson` was the originating example of that, hence why we have a test for it.

---

_Comment by @charliermarsh on 2024-02-02 22:01_

Could this be a path conversion error somewhere?

---

_Assigned to @konstin by @charliermarsh on 2024-02-05 15:40_

---

_Comment by @charliermarsh on 2024-02-05 15:40_

@konstin - I'm gonna assign to you.

---

_Referenced in [astral-sh/uv#1231](../../astral-sh/uv/pulls/1231.md) on 2024-02-05 20:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-11 19:20_

---

_Unassigned @konstin by @charliermarsh on 2024-02-11 19:20_

---

_Comment by @charliermarsh on 2024-02-11 19:27_

This works fine on my Windows machine.

---

_Comment by @charliermarsh on 2024-02-11 19:32_

Oh sorry, let me see what happens when I try to build from source.

---

_Comment by @charliermarsh on 2024-02-11 19:37_

Okay, reproduced.

---

_Comment by @charliermarsh on 2024-02-11 19:43_

I'll take over.

---

_Comment by @charliermarsh on 2024-02-11 20:06_

I think this has something to do with setting the current working directory.

---

_Comment by @charliermarsh on 2024-02-11 20:27_

It's possible this issue will drive me insane though.

---

_Referenced in [astral-sh/uv#1277](../../astral-sh/uv/pulls/1277.md) on 2024-02-12 00:47_

---

_Closed by @charliermarsh on 2024-02-12 00:51_

---
