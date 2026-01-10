```yaml
number: 16634
title: "`uv build` crashes with hatchling when different items in sdist and wheel"
type: issue
state: closed
author: rzuckerm
labels:
  - question
assignees: []
created_at: 2025-11-07T16:00:39Z
updated_at: 2025-11-07T20:54:15Z
url: https://github.com/astral-sh/uv/issues/16634
synced_at: 2026-01-10T03:23:55Z
```

# `uv build` crashes with hatchling when different items in sdist and wheel

---

_Issue opened by @rzuckerm on 2025-11-07 16:00_

### Summary

Here is my original `pyproject.toml` in poetry:
```toml
[tool.poetry]
name = "test_pkg"
version = "0.1.dev"
description = ""
authors = ["rzuckerm"]
include = [
    {path = "a.txt", format = "sdist"},
    {path = "b.txt", format = "wheel"}
]

[tool.poetry.dependencies]
python = "^3.10"

[build-system]
requires = ["poetry_core>=1.9.1,<2.0.0"]
build-backend = "poetry.core.masonry.api"
```

Here is my project structure:
```
.
├── a.txt
├── b.txt
├── poetry.lock
├── pyproject.toml
└── test_pkg
    └── __init__.py
```
Yes, I am aware that `a.txt` and `b.txt` should be in `test_pkg`, but I'm trying to cover corner cases that I have observed in our developer community.

If I do `poetry build`, I get this for the sdist:
```console
$ tar tf dist/*.gz | sort
test_pkg-0.1.dev0/PKG-INFO
test_pkg-0.1.dev0/a.txt
test_pkg-0.1.dev0/pyproject.toml
test_pkg-0.1.dev0/test_pkg/__init__.py
```
and this for the wheel:
```console
$ zipinfo -1 dist/*.whl
b.txt
test_pkg-0.1.dev0.dist-info/METADATA
test_pkg-0.1.dev0.dist-info/RECORD
test_pkg-0.1.dev0.dist-info/WHEEL
test_pkg/__init__.py
```

I converted my `pyproject.toml` to uv using [migrate-to-uv](https://github.com/mkniewallner/migrate-to-uv) plus some edits since that tool is buggy:
```toml
[project]
name = "test_pkg"
version = "0.1.dev"
description = ""
authors = [{ name = "rzuckerm" }]
requires-python = ">=3.10,<4"

[tool.hatch.build.targets.sdist]
include = ["test_pkg", "a.txt"]

[tool.hatch.build.targets.wheel]
include = ["test_pkg"]

[tool.hatch.build.targets.wheel.force-include]
"b.txt" = "b.txt"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
If I just do `uv build`, then uv builds the sdist and then builds the wheel from the sdist:
```console
$ uv build
Building source distribution...
Building wheel from source distribution...
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/rzuckerm/.cache/uv/builds-v0/.tmpPjaY0A/lib/python3.12/site-packages/hatchling/build.py", line 58, in build_wheel
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['standard'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/rzuckerm/.cache/uv/builds-v0/.tmpPjaY0A/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 155, in build
    artifact = version_api[version](directory, **build_data)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/rzuckerm/.cache/uv/builds-v0/.tmpPjaY0A/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 477, in build_standard
    for included_file in self.recurse_included_files():
                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/rzuckerm/.cache/uv/builds-v0/.tmpPjaY0A/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 177, in recurse_included_files
    yield from self.recurse_forced_files(self.config.get_force_include())
  File "/home/rzuckerm/.cache/uv/builds-v0/.tmpPjaY0A/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 237, in recurse_forced_files
    raise FileNotFoundError(msg)
FileNotFoundError: Forced include not found: /home/rzuckerm/.cache/uv/sdists-v9/.tmpWKlryw/test_pkg-0.1.dev0/b.txt
  × Failed to build `/tmp/test-pkg`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```
However, if I run `uv build --sdist` and `uv build --wheel` separately, then I seem to get a result that is similar to poetry. Here is the sdist:
```console
$ tar tf dist/*.gz | sort
test_pkg-0.1.dev0/.gitignore
test_pkg-0.1.dev0/PKG-INFO
test_pkg-0.1.dev0/a.txt
test_pkg-0.1.dev0/pyproject.toml
test_pkg-0.1.dev0/test_pkg/__init__.py
```
Here is the wheel:
```console
$ zipinfo -1 dist/*.whl | sort
b.txt
test_pkg-0.1.dev0.dist-info/METADATA
test_pkg-0.1.dev0.dist-info/RECORD
test_pkg-0.1.dev0.dist-info/WHEEL
test_pkg/__init__.py
```

Except for the addition of `.gitignore` in the sdist that hatchling insists on adding, the files are identical.

### Platform

Ubuntu 24.04 amd64

### Version

uv 0.9.7

### Python version

Python 3.12.11

---

_Label `bug` added by @rzuckerm on 2025-11-07 16:00_

---

_Comment by @zanieb on 2025-11-07 20:49_

This matches our documentation, we build the wheel from the sdist by default (as is best practice because it catches mistakes like a missing file in the sdist). By using `--wheel`, you opt-in to building the wheel directly from the source tree instead.

> `uv build` accepts a path to a directory or source distribution, which defaults to the current working
directory.
> 
> By default, if passed a directory, `uv build` will build a source distribution ("sdist") from the source
directory, and a binary distribution ("wheel") from the source distribution.
>
> `uv build --sdist` can be used to build only the source distribution, `uv build --wheel` can be used to
build only the binary distribution, and `uv build --sdist --wheel` can be used to build both
distributions from source.
> 
> If passed a source distribution, `uv build --wheel` will build a wheel from the source distribution.

---

_Label `bug` removed by @zanieb on 2025-11-07 20:50_

---

_Label `question` added by @zanieb on 2025-11-07 20:50_

---

_Comment by @rzuckerm on 2025-11-07 20:54_

@zanieb It looks like `uv build --sdist --wheel` does what I want without having to run `uv build` twice. Thanks!

---

_Closed by @rzuckerm on 2025-11-07 20:54_

---
