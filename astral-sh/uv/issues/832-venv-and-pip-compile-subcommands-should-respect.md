```yaml
number: 832
title: "venv and pip-compile subcommands should respect `requires-python`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-01-08T13:46:46Z
updated_at: 2024-07-09T12:47:49Z
url: https://github.com/astral-sh/uv/issues/832
synced_at: 2026-01-10T05:31:36Z
```

# venv and pip-compile subcommands should respect `requires-python`

---

_Issue opened by @konstin on 2024-01-08 13:46_

Running `puffin venv` in a directory with a pyproject.toml in it (or in any parent) with e.g.
```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.11,<3.13"
```
should default to the highest available python matching the specifiers.

Currently, when `python` is 3.10, it will create an incompatible venv with python 3.10 instead.

The same goes for the `pip-compile` with `pyproject.toml`. Currently, it will resolve for versions mismatching `requires-python`. It should also try to find a matching `python3.x` when no python version is activated by iterating over matching supported `python3.x` until it finds one in `PATH`.

The `venv` subcommand should also get a `--resolution lowest` option to force the lowest compatible python version instead. This is useful to ensure compatibility in CI.

---

_Label `enhancement` added by @konstin on 2024-01-08 13:46_

---

_Label `wish` added by @charliermarsh on 2024-01-08 14:34_

---

_Comment by @astrofrog on 2024-03-21 12:34_

I just ran into this - to give a concrete example, if I have a ``requirements.in`` file containing:

```
numpy
```

and run:

```
uv pip compile requirements.in --resolution=lowest
```

it will try and build an ancient version of Numpy that isn't Python 3-compatible:

```
error: Failed to download and build: numpy==1.3.0
  Caused by: Failed to build: numpy==1.3.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/tom/Library/Caches/uv/.tmpm0vE0w/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/tom/Library/Caches/uv/.tmpm0vE0w/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/Users/tom/Library/Caches/uv/.tmpm0vE0w/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/tom/Library/Caches/uv/.tmpm0vE0w/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 62
    print " --- Could not run svn info --- "
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: Missing parentheses in call to 'print'. Did you mean print(...)?
```

---

_Comment by @matterhorn103 on 2024-06-11 23:07_

This would also be very useful to me – was trying to get someone unfamiliar with Python up and running using uv (because it makes everything so easy!) and couldn't just tell them to do `uv venv` because I couldn't rely on `python3` returning the most recent Python interpreter on the system on Linux.

The toolchains on the horizon will no doubt make this sort of thing a lot easier but even so, it would be nice if the user is prevented from creating a venv with a Python version that doesn't meet the minimum requirements of the project.

---

_Comment by @matterhorn103 on 2024-06-12 14:35_

It seems that `uv tool run` also doesn't respect `requires-python` and just uses whatever interpreter it can find.

---

_Comment by @charliermarsh on 2024-06-12 14:37_

If you have a Python requirement, I think you should be providing that with `-p`? I don't know that it's right for `tool run` to respect `requires-python`. `tool run` is a global install, but `requires-python` is a project-centric concept.

---

_Comment by @zanieb on 2024-06-12 14:53_

Well, `tool run` will read project metadata for project-level tools. It might make sense for it to respect project metadata (like `requires-python`) by default? I'll definitely need to think about that some.

---

_Comment by @matterhorn103 on 2024-06-12 16:25_

> If you have a Python requirement, I think you should be providing that with `-p`? I don't know that it's right for `tool run` to respect `requires-python`. `tool run` is a global install, but `requires-python` is a project-centric concept.

I guess it makes sense to make that distinction.

At the same time, I can't think when you would actively _want_ to use a different Python version for a tool as for your project? (Barring the tool not being available for that version, in which case you're surely unlikely to be using it.) Meanwhile I can think of a couple of situations where it might cause an issue by not being synced:

1. My problematic use case was running `uv tool run pyinstaller foo.py`, because pyinstaller builds the project using the interpreter its run with and doesn't lok at `pyproject.toml` at all (which you could argue is a flaw with pyinstaller)
2. If the installed version of a linter/parser like black or ruff is one released when the Python version used for the project was not yet released, the linter/parser will presumably not be equipped to handle the newer syntax of the project

> `tool run` is a global install

But is it really global? If I create a venv using `uv venv -p 3.12` then `tool run` uses the 3.12 interpreter, not my default system Python (3.11). Even if I don't activate the venv. So it already adapts to the current directory's contents to some extent. As well as, as @zanieb says, adapting to `[tool]` tables in `pyproject.toml`.

---

_Comment by @zanieb on 2024-06-12 18:14_

>  If I create a venv using uv venv -p 3.12 then tool run uses the 3.12 interpreter, not my default system Python (3.11). Even if I don't activate the venv.

This is probably not intentional. We're just following our standard interpreter discovery order. Arguably that's weird behavior but anyway yeah it seems nice to find an interpreter relevant to where you're working.

`uv tool install` is intended to be more global and should definitely _not_ respect anything in the working directory or active virtual environment.

---

_Comment by @konstin on 2024-07-09 12:47_

`uv sync` (currently in preview) creates a venv respects the `requires-python` in `pyproject.toml`, i don't think it makes sense changing this in `uv venv` still.

---

_Closed by @konstin on 2024-07-09 12:47_

---
