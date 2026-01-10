```yaml
number: 406
title: Treat python version as a dependency when resolving
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2023-11-13T08:32:25Z
updated_at: 2024-01-03T15:20:46Z
url: https://github.com/astral-sh/uv/issues/406
synced_at: 2026-01-10T05:40:31Z
```

# Treat python version as a dependency when resolving

---

_Issue opened by @konstin on 2023-11-13 08:32_

Sometimes the resolution is impossible because a version of a package can only be installed in a more recent python version than the user has. In the example below, pandas 2.1 requires at least python 3.9, while the user has 3.8.

My suggestion: Add a virtual python dependency with root depending on the user specified version. Set the python dependency of each version to the `requires-python` of the sdist or that of the union of the wheel if there is no sdist.

### puffin

```console
$ VIRTUAL_ENV=.venv38 cargo run --bin puffin -q -- pip-compile pandas-2.1.in 
error: Failed to build distribution: pandas-2.1.0.tar.gz
  Caused by: Build backend failed to determine extras requires with `get_requires_for_build_wheel`:
--- stdout:
+ meson setup /tmp/.tmpoW2Ldn/extracted/pandas-2.1.0 /tmp/.tmpoW2Ldn/extracted/pandas-2.1.0/.mesonpy-i9atvlaf/build -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --vsenv --native-file=/tmp/.tmpoW2Ldn/extracted/pandas-2.1.0/.mesonpy-i9atvlaf/build/meson-python-native-file.ini
The Meson build system
Version: 1.2.3
Source dir: /tmp/.tmpoW2Ldn/extracted/pandas-2.1.0
Build dir: /tmp/.tmpoW2Ldn/extracted/pandas-2.1.0/.mesonpy-i9atvlaf/build
Build type: native build

../../meson.build:5:13: ERROR: Command `/home/konsti/projects/puffin/.venv/bin/python generate_version.py --print` failed with status 1.

A full log can be found at /tmp/.tmpoW2Ldn/extracted/pandas-2.1.0/.mesonpy-i9atvlaf/build/meson-logs/meson-log.txt
--- stderr:

---
```

We should have never picked this sdist!

### puffin after #398

```
$ VIRTUAL_ENV=.venv38 cargo run --bin puffin -q -- pip-compile pandas-2.1.in 
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pandas available matching ==2.1 and root depends on pandas==2.1,
      version solving failed.
```

I can go to pypi and see that there is a pandas 2.1.2 there, why doesn't puffin use it?

### pip/pip-tools
(also used by pip-tools, but with a backtrace below):

```console
$ .venv38/bin/pip install pandas==2.1
ERROR: Ignored the following versions that require a different python version: 2.1.0 Requires-Python >=3.9; 2.1.0rc0 Requires-Python >=3.9; 2.1.1 Requires-Python >=3.9; 2.1.2 Requires-Python >=3.9; 2.1.3 Requires-Python >=3.9
ERROR: Could not find a version that satisfies the requirement pandas==2.1 (from versions: 0.1, 0.2, 0.3.0, 0.4.0, 0.4.1, 0.4.2, 0.4.3, 0.5.0, 0.6.0, 0.6.1, 0.7.0, 0.7.1, 0.7.2, 0.7.3, 0.8.0, 0.8.1, 0.9.0, 0.9.1, 0.10.0, 0.10.1, 0.11.0, 0.12.0, 0.13.0, 0.13.1, 0.14.0, 0.14.1, 0.15.0, 0.15.1, 0.15.2, 0.16.0, 0.16.1, 0.16.2, 0.17.0, 0.17.1, 0.18.0, 0.18.1, 0.19.0, 0.19.1, 0.19.2, 0.20.0, 0.20.1, 0.20.2, 0.20.3, 0.21.0, 0.21.1, 0.22.0, 0.23.0, 0.23.1, 0.23.2, 0.23.3, 0.23.4, 0.24.0, 0.24.1, 0.24.2, 0.25.0, 0.25.1, 0.25.2, 0.25.3, 1.0.0, 1.0.1, 1.0.2, 1.0.3, 1.0.4, 1.0.5, 1.1.0, 1.1.1, 1.1.2, 1.1.3, 1.1.4, 1.1.5, 1.2.0, 1.2.1, 1.2.2, 1.2.3, 1.2.4, 1.2.5, 1.3.0, 1.3.1, 1.3.2, 1.3.3, 1.3.4, 1.3.5, 1.4.0rc0, 1.4.0, 1.4.1, 1.4.2, 1.4.3, 1.4.4, 1.5.0rc0, 1.5.0, 1.5.1, 1.5.2, 1.5.3, 2.0.0rc0, 2.0.0rc1, 2.0.0, 2.0.1, 2.0.2, 2.0.3)
ERROR: No matching distribution found for pandas==2.1
```

Why does pip pretend there is no pandas 2.1.2 when i can clearly see it on pypi, is this a caching bug?

### poetry:

```toml
[tool.poetry]
name = "a"
version = "0.1.0"
description = ""
authors = ["konstin <konstin@mailbox.org>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "~3.8"
pandas = "^2.1"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

```
$ VIRTUAL_ENV=.venv38 poetry update
Updating dependencies
Resolving dependencies... (0.2s)

The current project's Python requirement (>=3.8,<3.9) is not compatible with some of the required packages Python requirement:
  - pandas requires Python >=3.9, so it will not be satisfied for Python >=3.8,<3.9
  - pandas requires Python >=3.9, so it will not be satisfied for Python >=3.8,<3.9
  - pandas requires Python >=3.9, so it will not be satisfied for Python >=3.8,<3.9
  - pandas requires Python >=3.9, so it will not be satisfied for Python >=3.8,<3.9

Because no versions of pandas match >2.1,<2.1.1 || >2.1.1,<2.1.2 || >2.1.2,<2.1.3 || >2.1.3,<3.0
 and pandas (2.1.0) requires Python >=3.9, pandas is forbidden.
And because pandas (2.1.1) requires Python >=3.9
 and pandas (2.1.2) requires Python >=3.9, pandas is forbidden.
So, because pandas (2.1.3) requires Python >=3.9
 and a depends on pandas (^2.1), version solving failed.

  • Check your dependencies Python requirement: The Python requirement can be specified via the `python` or `markers` properties
    
    For pandas, a possible solution would be to set the `python` property to "<empty>"
    For pandas, a possible solution would be to set the `python` property to "<empty>"
    For pandas, a possible solution would be to set the `python` property to "<empty>"
    For pandas, a possible solution would be to set the `python` property to "<empty>"

    https://python-poetry.org/docs/dependency-specification/#python-restricted-dependencies,
    https://python-poetry.org/docs/dependency-specification/#using-environment-markers
```

> The current project's Python requirement (>=3.8,<3.9) is not compatible

> pandas requires Python >=3.9

I see, i have to change the python requirement in pyproject.toml (or use an older pandas version)

---

_Label `error messages` added by @konstin on 2023-11-13 08:32_

---

_Comment by @konstin on 2023-11-13 12:16_

Update: Wheels also have a `requires-python` we have to consider, this also answers the question what we do when there is no source dist

---

_Comment by @charliermarsh on 2023-11-13 14:32_

Do you know how the `requires-python` on a wheel differs from its tags?

---

_Comment by @konstin on 2023-11-13 15:38_

In most cases, the tag is `py3-none-any`, while `requires-python` specifies a minimum python minor version such as `>=3.8`

---

_Comment by @charliermarsh on 2024-01-02 21:10_

I will give this a shot.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-02 22:23_

---

_Closed by @charliermarsh on 2024-01-03 15:20_

---
