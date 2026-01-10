```yaml
number: 15783
title: Configuring uv to source Python from conda
type: issue
state: open
author: thomasaarholt
labels:
  - question
assignees: []
created_at: 2025-09-11T07:26:09Z
updated_at: 2025-09-11T07:41:28Z
url: https://github.com/astral-sh/uv/issues/15783
synced_at: 2026-01-10T03:23:54Z
```

# Configuring uv to source Python from conda

---

_Issue opened by @thomasaarholt on 2025-09-11 07:26_

### Question

Due to compliance reasons, at my work (Microsoft) we have to get our python binaries from conda. We'd really like to use the uv project functionality, however, so I am wondering if it is possible to point uv at several conda environments so that those python versions are used.

This is what I have tried:

I notice that if I call `uv python list`, the returned list begins with:
```bash
❯ uv python list
cpython-3.14.0rc2-windows-x86_64-none                 <download available>
cpython-3.14.0rc2+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.7-windows-x86_64-none                    <download available>
cpython-3.13.7+freethreaded-windows-x86_64-none       <download available>
cpython-3.13.5-windows-x86_64-none                    AppData\Local\miniconda3\python.exe      # <------ Note this one
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.12.9-windows-x86_64-none                    AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe
cpython-3.11.13-windows-x86_64-none                   <download available>
<snip>
```

Our users (this is a data science monorepo with several projects) need to have one version each of Python 3.10-3.13 available.

The base conda environment, which happened to use python 3.13, is installed at `C:\Users\thaarholt\AppData\Local\miniconda3\python.exe`.
I can use the command `conda create --name <NAME> python=3.11` to create a new additional python environment, at 3.11, which will be created at `C:\Users\thaarholt\AppData\Local\miniconda3\envs\<NAME>`. That folder contains amongst other files, a `python.exe`.

I thought to point the env variable `UV_PYTHON_INSTALL_DIR` at the `...\envs` folder together with `--no-managed-python`, and hoped that that would be enough:
```powershell
❯ $env:UV_PYTHON_INSTALL_DIR = "C:\Users\thaarholt\AppData\Local\miniconda3\envs"
❯ uv python dir
C:\Users\thaarholt\AppData\Local\miniconda3\envs
```

But when I look for the python versions that are available, 3.11 is not found. Yet the base environment still is, oddly.
```
❯ uv python list
<snip>
cpython-3.13.5-windows-x86_64-none                    C:\Users\thaarholt\AppData\Local\miniconda3\python.exe   # <------ Why is this one still found?
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.11.13-windows-x86_64-none                   <download available>      # <------ Still no python 3.11 found!
<snip>
```

And then just to be sure, when I try to make a new environment, the 3.11 version is not found:

```powershell
❯ uv venv --no-managed-python --python 311
error: No interpreter found for Python 3.11 in registry or search path

hint: A managed Python download is available for Python 3.11, but the Python preference is set to 'only system'
```

Am I using this variable wrong? Does anyone have any advice?

### Platform

Windows 11 x86_65

### Version

uv 0.8.17 (10960bc13 2025-09-10)

---

_Label `question` added by @thomasaarholt on 2025-09-11 07:26_

---

_Comment by @konstin on 2025-09-11 07:41_

`UV_PYTHON_INSTALL_DIR` is for uv's own managed Pythons, to change the download location, it doesn't find other Python in that location.

To make the conda Pythons available to uv, the easiest way is to make each one available in `PATH`. uv searches in `PATH`, and if the requested version is available there, it uses it instead of trying a download a managed Python; it will treat conda Python like system installations.

Conda is very different from the environment we usually use, with conda having a separate activation mechanism, a different package and its own index format (https://github.com/astral-sh/uv/issues/1703).

---
