---
number: 8721
title: "how to specify the venv when running `uv pip install`"
type: issue
state: closed
author: KotlinIsland
labels:
  - question
assignees: []
created_at: 2024-10-31T12:29:20Z
updated_at: 2024-10-31T13:50:27Z
url: https://github.com/astral-sh/uv/issues/8721
synced_at: 2026-01-07T13:12:18-06:00
---

# how to specify the venv when running `uv pip install`

---

_Issue opened by @KotlinIsland on 2024-10-31 12:29_

i'm trying to write a [pyprojectx](https://pyprojectx.github.io/) install script for basedmypy where the user can pass `--python 3.8` etc, but i am struggling to figure out how to do this

i originally had:
```toml
[tool.pyprojectx.aliases]
install = ["uv venv", "uv pip install -r test-requirements.txt -e ."]
```

but when this was invoked:
```console
> ./px install --python 3.8
px install --python 3.8 .venv8                                                                                                                                                                                                                                                                                                                                                                                       Using CPython 3.8.10 interpreter at: C:\Users\AMONGUS\AppData\Local\Programs\Python\Python38\python.exe
Creating virtual environment at: .venv8
Activate with: .venv8\Scripts\activate
error: C:\Users\AMONGUS\projects\basedmypy\.venv8 does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

which makes sense, `uv pip install` expects packages, not a venv directory, so i switched to writing a separate python script, along the lines of:

```py
# psuedo code, for brevity
from subprocess import check_call

# parse args ...
check_call(["uv", "venv" "--python", args.python, f".venv{args.python}"])
check_call(["uv", "pip" "install", "-r", "test-requirements.txt", "-e", ".", "--python", args.python])
```

this has the issue that `uv pip` doesn't know anything about the newly created venv...


so i've tried all these things, but i can't seem to get anywhere, am i missing something here?

```console
> uv venv --python 3.8 .venv8
Using CPython 3.8.10 interpreter at: C:\Users\AMONGUS\AppData\Local\Programs\Python\Python38\python.exe
Creating virtual environment at: .venv8
Activate with: .venv8\Scripts\activate
> uv pip install -r test-requirements.txt -e . --python 3.8
error: No virtual environment found for Python 3.8; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
> uv pip install -r test-requirements.txt -e . --python 3.8 --prefix .venv8
error: No virtual environment found for Python 3.8; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
> uv pip install -r test-requirements.txt -e . --python 3.8 --target .venv8/Lib/site-packages
error: No virtual environment found for Python 3.8; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

ideally, i want a simple command that will create a venv with a specific name and install dependencies in it:

```console
> uv pip install <blah blah> --python 3.8 --venv .venv8
```

better yet, when specifying python version, `uv` could have a set way of naming and identifying the different venvs

or maybe, you think that is a bad ux, and everyone should just recreate the same `.venv` every time they want to switch python versions then i guess this issue is invalid

with basedmypy specifically, i need to switch Python versions constantly

---

_Comment by @charliermarsh on 2024-10-31 12:56_

The second call should use `--python f".venv{args.python}"`, not `--python args.python`.

---

_Label `question` added by @charliermarsh on 2024-10-31 12:59_

---

_Comment by @KotlinIsland on 2024-10-31 13:10_

ah, that makes sense, thanks

---

_Comment by @KotlinIsland on 2024-10-31 13:13_

okay thanks, so that does resolve my question and my situation. the only thing left is the request for a shorthand, instead of needing to write a script

---

_Comment by @charliermarsh on 2024-10-31 13:32_

I don't think we're likely to add a mechanism to create a virtualenv as part of a `pip install` command (if that's what you're referring to), since the commands are already composable?

```
uv venv --python 3.8 .venv-3.8 && uv pip install -r test-requirements.txt --python .venv-3.8
```

---

_Comment by @KotlinIsland on 2024-10-31 13:49_

yeah, okay. just can't easily abstract that into an alias, all good, thanks so much for responding as always 😁

---

_Closed by @KotlinIsland on 2024-10-31 13:49_

---

_Comment by @charliermarsh on 2024-10-31 13:50_

No problem, glad I could help, appreciate your comments :)

---
