---
number: 1656
title: Virtualenv creation is failing on Python embedded versions
type: issue
state: closed
author: Pixel-Minions
labels:
  - bug
  - windows
  - virtualenv
assignees: []
created_at: 2024-02-18T16:49:54Z
updated_at: 2024-12-08T02:31:51Z
url: https://github.com/astral-sh/uv/issues/1656
synced_at: 2026-01-07T13:12:16-06:00
---

# Virtualenv creation is failing on Python embedded versions

---

_Issue opened by @Pixel-Minions on 2024-02-18 16:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi,

When I try to create a virtualenv from a Python Embedded version on Windows I get the following error:

```cmd
Using Python 3.7.9 interpreter at C:\Users\alex\Documents\Python\python-3.7.9-embed-amd64\python.exe
Creating virtualenv at: .venv
uv::venv::creation

  x Failed to create virtualenv
  |-> failed to copy file from C:\Users\alex\Documents\Python\python-3.7.9-embed-amd64\Lib\venv\scripts\nt\python.exe
  |   to \\?\D:\testing\uv\.venv\Scripts\python.exe
  `-> The system cannot find the path specified. (os error 3)
```

The path: `C:\Users\alex\Documents\Python\python-3.7.9-embed-amd64\Lib\venv\scripts\nt\python.exe` doesn't exist.

The error is happening on Windows 10 and uv-0.1.4.

How to recreate the issue:
1. Download an embedded version of Python.
2. Unzip it on any folder.
3. Append it to the PATH and make sure it is the Python version by default.
4. run  `uv venv`.
5. Fails.

Best,




---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Referenced in [astral-sh/uv#1779](../../astral-sh/uv/issues/1779.md) on 2024-02-23 17:16_

---

_Comment by @Pixel-Minions on 2024-02-26 16:54_

Hey guys,

I wonder if there is any plans to support embedded version, I would really like to bring UV to the company and replace all the libraries we have with Poetry, but sadly, without being able to work with embedded versions, it seems like it won't work.

---

_Comment by @konstin on 2024-02-26 17:02_

What exactly do you mean by embedded version of python, could you link me to a download page?

---

_Comment by @Pixel-Minions on 2024-02-26 17:03_

https://www.python.org/ftp/python/3.11.8/python-3.11.8-embed-amd64.zip

https://www.python.org/downloads/release/python-3118/

@konstin 

---

_Label `bug` added by @konstin on 2024-02-26 18:32_

---

_Comment by @konstin on 2024-02-27 13:13_

Thanks!

I've checked with `virtualenv` and apparently the strategy for supporting embeddable python version is to check for the shim and if it doesn't exist, copy all files: https://github.com/pypa/virtualenv/blob/cad550030ae77e181a1d7c328742a97f2880ef9b/src/virtualenv/create/via_global_ref/builtin/cpython/cpython3.py#L58-L68

---

_Label `windows` added by @konstin on 2024-02-27 13:13_

---

_Comment by @swz-git on 2024-03-29 19:43_

When running `uv.exe venv testvenv --python .\python-3.11.8-embed-amd64\python.exe`, I get
```batch
Using Python 3.11.8 interpreter at: python-3.11.8-embed-amd64\python.exe
Creating virtualenv at: testvenv
uv::venv::creation

  x Failed to create virtualenv
  |-> failed to copy file from \\?\C:\Users\simlin1\Downloads\python-3.11.8-embed-amd64\venvwlauncher.exe to
  |   \\?\C:\Users\simlin1\Downloads\testvenv\Scripts\python.exe
  `-> Det går inte att hitta filen. (os error 2)       
```

---

_Comment by @Pixel-Minions on 2024-04-19 14:36_

Yep, same error here. It seems uv is expecting specific files to be there. With virtualenv, there is no problem however.

---

_Comment by @charliermarsh on 2024-04-19 15:06_

For starters can we get a minimal reproducible example for this? Like a Dockerfile that I can run to understand the current behavior and what the structure of these embedded Pythons looks like?

---

_Comment by @konstin on 2024-04-19 16:34_

I can reproduce this when downloading the embedded python from (https://www.python.org/ftp/python/3.11.8/python-3.11.8-embed-amd64.zip) and using it for `uv venv -p path\to\embedded\python`. The embedded python i looked at was a flat directory which we need to copy to support this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-20 03:00_

---

_Comment by @charliermarsh on 2024-04-20 03:07_

I think I see what needs to be changed, and it looks relatively (?) straightforward. Though there are some comments from Paul Moore around this not being an intended use-case for the embedded distribution: https://github.com/pypa/virtualenv/issues/2368#issuecomment-1168574713.

---

_Comment by @charliermarsh on 2024-04-20 03:19_

Can anyone just clarify for me what the motivating use-case is for using the embedded Pythons here?

---

_Comment by @Pixel-Minions on 2024-04-20 05:21_

The comment you mentioned makes sense, there are ways around to create portable versions of python that might work with uv. I will try the nuget or winpython. 

The reason for the embedded version is that, after some minor changes to the .pht file and using virtualenv, you can create portable virtual environment, it doesn't get linked to the interpreter source like if you would use a standard Python installation.

---

_Referenced in [astral-sh/uv#3161](../../astral-sh/uv/pulls/3161.md) on 2024-04-20 15:52_

---

_Comment by @charliermarsh on 2024-04-20 22:04_

I added virtualenv-like support in https://github.com/astral-sh/uv/pull/3161.

---

_Closed by @charliermarsh on 2024-04-22 17:34_

---

_Comment by @joetristano on 2024-12-08 02:31_

Can uv install an embedded python version on windows?

---
