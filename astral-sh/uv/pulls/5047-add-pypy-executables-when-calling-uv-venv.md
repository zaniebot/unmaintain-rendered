```yaml
number: 5047
title: "Add `pypy` executables when calling `uv venv`"
type: pull_request
state: merged
author: silvanocerza
labels:
  - enhancement
assignees: []
merged: true
base: main
head: venv-add-pypy-exes
created_at: 2024-07-14T14:46:10Z
updated_at: 2024-07-15T19:00:50Z
url: https://github.com/astral-sh/uv/pull/5047
synced_at: 2026-01-10T13:42:52Z
```

# Add `pypy` executables when calling `uv venv`

---

_Pull request opened by @silvanocerza on 2024-07-14 14:46_

## Summary

Should fix #2092.

This PR changes `uv venv` so it also creates symlinks to `pypy` on Unix and copies executables on Windows when creating a new environment using PyPy.

I found a bit of discrepancy between creation of a venv using `python` and `uv`, as using `python` brings all the executables with it. While `uv` brings only those without any version number, at least on Windows. The behaviour is different on Unix as we take the versioned symlinks too.

Some examples below.

`python -m venv` generates the following `Scripts` folder.
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         7/14/2024     15:41           2031 activate
-a----         7/14/2024     15:41           1029 activate.bat
-a----         7/14/2024     15:41           9033 Activate.ps1
-a----         7/14/2024     15:41            393 deactivate.bat
-a----         7/14/2024     15:40          27648 libffi-8.dll
-a----         7/14/2024     15:41       44290560 libpypy3.10-c.dll
-a----         7/14/2024     15:41         108424 pip.exe
-a----         7/14/2024     15:41         108424 pip3.10.exe
-a----         7/14/2024     15:41         108424 pip3.exe
-a----         7/14/2024     15:41          79360 pypy.exe
-a----         7/14/2024     15:41          79360 pypy3.10.exe
-a----         7/14/2024     15:41          79360 pypy3.10w.exe
-a----         7/14/2024     15:41          79360 pypy3.exe
-a----         7/14/2024     15:41          79360 pypyw.exe
-a----         7/14/2024     15:41          79360 python.exe
-a----         7/14/2024     15:41          79360 python3.10.exe
-a----         7/14/2024     15:41          79360 python3.exe
-a----         7/14/2024     15:41          79360 pythonw.exe
```

`uv venv` instead generates this. 
```
-a----         7/14/2024     16:27           3360 activate
-a----         7/14/2024     16:27           2251 activate.bat
-a----         7/14/2024     16:27           2627 activate.csh
-a----         7/14/2024     16:27           4191 activate.fish
-a----         7/14/2024     16:27           3875 activate.nu
-a----         7/14/2024     16:27           2766 activate.ps1
-a----         7/14/2024     16:27           2378 activate_this.py
-a----         7/14/2024     16:27           1728 deactivate.bat
-a----         7/13/2024     19:19          27648 libffi-8.dll
-a----         7/13/2024     19:19       44290560 libpypy3.10-c.dll
-a----         7/14/2024     16:27           1215 pydoc.bat
-a----         7/13/2024     19:19          79360 pypy.exe
-a----         7/13/2024     19:19          79360 pypyw.exe
-a----         7/13/2024     19:19          79360 python.exe
-a----         7/13/2024     19:19          79360 pythonw.exe
```

## Test Plan

To verify the correct behaviour:

1. Download and install PyPy from [official website](https://www.pypy.org/download.html)
2. Call `uv venv -p <path_to_pypy_>`
3. Run `.\.venv\Scripts\activate` on Windows or `./.venv/Scripts/activate` on Unix
4. Run `pypy`

I thought of writing some automated tests but I couldn't rely on `uv python install` command to install PyPy as it's not in the list of installable Python builds.


---

_@silvanocerza reviewed on 2024-07-14 14:48_

---

_Review comment by @silvanocerza on `crates/uv-virtualenv/src/bare.rs`:357 on 2024-07-14 14:48_

I have no idea whether this is correct or not.

Reading a comment below I seemed to understand that the shims changed name in Python 3.13 but after installing it doesn't seem like it.

Also I think this shouldn't apply to PyPy for the time being since it supports up to Python 3.10 only.

---

_Renamed from "Add `pypy ` executables when calling `uv venv`" to "Add `pypy` executables when calling `uv venv`" by @silvanocerza on 2024-07-14 14:50_

---

_Assigned to @zanieb by @zanieb on 2024-07-14 15:29_

---

_Comment by @zanieb on 2024-07-14 16:13_

Here's a commit showing how we could add a CI test for this until we add support to the actual test suite https://github.com/astral-sh/uv/pull/5048/commits/908eeb9a60ad1014dcc46d2d5e2e2f1b0cb5a365

I think on Windows we generally assume all Python executables don't contain version numbers cc @konstin, interesting that Python creates them in virtual environments.

---

_Comment by @konstin on 2024-07-14 16:30_

It looks like pypy always has `pythonx.y.exe` on windows, both in the archive and in the venv, and it's cpython-specific to omit the version number.

---

_Comment by @silvanocerza on 2024-07-15 16:12_

@zanieb new job added. Should I maybe add one for Windows too? I could actually do it all in a single job if I'm clever with `matrix`.

@konstin should I add the versioned binaries then?

---

_Comment by @zanieb on 2024-07-15 16:12_

I merged https://github.com/astral-sh/uv/pull/5048 so we can have most of the test changes separate from here — lmk if the conflicts are a problem.

---

_Comment by @zanieb on 2024-07-15 16:13_

We could add one for Windows but let's do it separately as a follow-up pull request, I suspect it'll be a little painful.

---

_Comment by @zanieb on 2024-07-15 16:13_

And yeah I'd add the versioned binaries to match the standard behavior.

---

_@zanieb reviewed on 2024-07-15 16:13_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/bare.rs`:357 on 2024-07-15 16:13_

Can you add a comment explaining this?

---

_Label `enhancement` added by @zanieb on 2024-07-15 16:14_

---

_Comment by @silvanocerza on 2024-07-15 16:16_

@zanieb cool, will add the binaries then. I already handled the conflicts. 👍 

---

_Comment by @silvanocerza on 2024-07-15 16:56_

PR ready. 👍 

---

_@zanieb approved on 2024-07-15 18:28_

---

_Merged by @zanieb on 2024-07-15 18:28_

---

_Closed by @zanieb on 2024-07-15 18:28_

---

_Comment by @zanieb on 2024-07-15 18:28_

Nice :)

---

_Branch deleted on 2024-07-15 19:00_

---
