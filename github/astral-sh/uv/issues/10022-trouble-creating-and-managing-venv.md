---
number: 10022
title: Trouble creating and managing venv
type: issue
state: closed
author: jiwidi
labels:
  - question
assignees: []
created_at: 2024-12-19T09:39:38Z
updated_at: 2024-12-28T09:10:37Z
url: https://github.com/astral-sh/uv/issues/10022
synced_at: 2026-01-07T13:12:18-06:00
---

# Trouble creating and managing venv

---

_Issue opened by @jiwidi on 2024-12-19 09:39_

Hi!

First of all sorry i've missed something. I went through the docs and i think i'm doing the right think but can figure out what's wrong.

So I'm trying to move out my project from conda to uv. It worked well when moving all my dockerfiles and scripts but when i try to create my local venv for quick development it can't pick up the venv i create.

I first create it with:
```
uv venv test_env
```
I get

```
Using CPython 3.12.7
Creating virtual environment at: test_env
Activate with: source test_env/bin/activate.fish
```

I activate it with 

```
source test_env/bin/activate.fish
```

Which runs fine, but when i try to install some requirements with

```
uv pip install --system -r requirements.txt
```

I get

```
Using Python 3.12.6 environment at: /opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12
error: The interpreter at /opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12 is externally managed, and indicates the following:

  To install Python packages system-wide, try brew install
  xyz, where xyz is the package you are trying to
  install.

  If you wish to install a Python library that isn't in Homebrew,
  use a virtual environment:

  python3 -m venv path/to/venv
  source path/to/venv/bin/activate
  python3 -m pip install xyz

  If you wish to install a Python application that isn't in Homebrew,
  it may be easiest to use 'pipx install xyz', which will manage a
  virtual environment for you. You can install pipx with

  brew install pipx

  You may restore the old behavior of pip by passing
  the '--break-system-packages' flag to pip, or by adding
  'break-system-packages = true' to your pip.conf file. The latter
  will permanently disable this error.

  If you disable this error, we STRONGLY recommend that you additionally
  pass the '--user' flag to pip, or set 'user = true' in your pip.conf
  file. Failure to do this can result in a broken Homebrew installation.

  Read more about this behavior here: <https://peps.python.org/pep-0668/>

Consider creating a virtual environment with `uv venv`.
```

Which is complaining about using an external 3.12 and not the recently created venv.


Any help appreciated ty


---

_Comment by @jiwidi on 2024-12-19 09:59_

To add.

I created a project within this repository a few days ago, when i look at the `.venv/pyvenv.cfg` file looks like:


```
home = /Users/jiwidi/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/bin
implementation = CPython
uv = 0.5.6
version_info = 3.12.7
include-system-site-packages = false
prompt = ml-cronjobs

```

So its not pointing to the `test_env/bin/activate.fish` environment created before. I see a python bin inside the .venv path too, should I be using that one then? Is this limited then for a project to have 1 venv? How do i manage them or change?


If I try sourcing that one 

```
source .venv/bin/activate.fish
```

and run my install 
```
uv pip install --system -r requirements.txt
```

I still get:

```

Using Python 3.12.6 environment at: /opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12
error: The interpreter at /opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12 is externally managed, and indicates the following:

  To install Python packages system-wide, try brew install
  xyz, where xyz is the package you are trying to
  install.

  If you wish to install a Python library that isn't in Homebrew,
  use a virtual environment:

  python3 -m venv path/to/venv
  source path/to/venv/bin/activate
  python3 -m pip install xyz

  If you wish to install a Python application that isn't in Homebrew,
  it may be easiest to use 'pipx install xyz', which will manage a
  virtual environment for you. You can install pipx with

  brew install pipx

  You may restore the old behavior of pip by passing
  the '--break-system-packages' flag to pip, or by adding
  'break-system-packages = true' to your pip.conf file. The latter
  will permanently disable this error.

  If you disable this error, we STRONGLY recommend that you additionally
  pass the '--user' flag to pip, or set 'user = true' in your pip.conf
  file. Failure to do this can result in a broken Homebrew installation.

  Read more about this behavior here: <https://peps.python.org/pep-0668/>

Consider creating a virtual environment with `uv venv`.
```

---

_Comment by @FishAlchemist on 2024-12-19 10:07_

Why do you need the ``--system`` flag? According to the documentation, this flag is supposed to ignore virtual environments.
```
--system
Install packages into the system Python environment.

By default, uv installs into the virtual environment in the current working directory or any parent directory. The --system option instructs uv to instead use the first Python found in the system PATH.

WARNING: --system is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.

May also be set with the UV_SYSTEM_PYTHON environment variable.
```

---

_Comment by @zanieb on 2024-12-21 06:45_

`home` in `pyvenv.cfg` is not the place where things are installed to, it's used to find the Python interpreter libraries. uv will error if you attempt to install into one of the environments at `/home/myuser/.local/share/uv/python`.

@FishAlchemist is correct, do not include the `--system` flag.

---

_Label `question` added by @zanieb on 2024-12-21 06:45_

---

_Comment by @jiwidi on 2024-12-28 09:10_

sorry for the late reply, the system did indeed work. Was using it from another command in our dockerfiles so mb there, ty!

---

_Closed by @jiwidi on 2024-12-28 09:10_

---
