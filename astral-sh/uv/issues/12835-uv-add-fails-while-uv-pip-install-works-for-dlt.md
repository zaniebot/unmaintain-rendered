---
number: 12835
title: "'uv add' fails while 'uv pip install' works for dlt[postgres]"
type: issue
state: closed
author: trin94
labels:
  - question
assignees: []
created_at: 2025-04-11T12:41:54Z
updated_at: 2025-05-27T19:27:07Z
url: https://github.com/astral-sh/uv/issues/12835
synced_at: 2026-01-10T01:25:25Z
---

# 'uv add' fails while 'uv pip install' works for dlt[postgres]

---

_Issue opened by @trin94 on 2025-04-11 12:41_

### Summary

Hey,

I'm currently in the process of migrating some of my projects from `poetry` to `uv`. With one in particular, I am having trouble with.

When using

```console
❯ uv add dlt[postgres]==1.3.0
```

it tries to build some of the dependencies. But when I use

```console
❯ uv pip install dlt[postgres]==1.3.0
```

or 

```console
❯ poetry add dlt[postgres]==1.3.0
```

it just works.

## Not Working: Logs with uv add ; uv version 0.6.14

```console
PycharmProjects/troubleshooting/uv-2
❯ uv init --python 3.12
Initialized project `uv-2`

uv-2 on  main [?] is 📦 v0.1.0 via 🐍 v3.13.2
❯ uv add dlt[postgres]==1.3.0
Using CPython 3.12.9 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
  × Failed to build `psycopg2cffi==2.9.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel` failed (exit status: 1)

      [stderr]
      /home/elias/.cache/uv/builds-v0/.tmpqcb1gF/lib64/python3.12/site-packages/setuptools/_distutils/dist.py:289:
      UserWarning: Unknown distribution option: 'test_suite'
        warnings.warn(msg)
      Error: pg_config executable not found.
      Please add the directory containing pg_config to the PATH.

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.
```

Of course, one solution would just be to add missing `pg_config` binary to the path and continue from there but this is definitely not something I expected.

## Working

<details>

<summary><b>Working:</b> Logs with Poetry (poetry version 1.8.3)</summary>

```console
PycharmProjects/troubleshooting/poetry-2
❯ poetry init

This command will guide you through creating your pyproject.toml config.

Package name [poetry-2]:
Version [0.1.0]:
Description []:
Author [..., n to skip]:  n
License []:
Compatible Python versions [^3.13]:  >=3.12,<3.13

Would you like to define your main dependencies interactively? (yes/no) [yes] n
Would you like to define your development dependencies interactively? (yes/no) [yes] n
Generated file

[tool.poetry]
name = "poetry-2"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = ">=3.12,<3.13"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes]

PycharmProjects/troubleshooting/poetry-2 is 📦 v0.1.0 via 🐍 v3.13.2 took 7s
❯ poetry add dlt[postgres]==1.3.0
The currently activated Python version 3.13.2 is not supported by the project (>=3.12,<3.13).
Trying to find and use a compatible version.
Using python3.12 (3.12.9)
Creating virtualenv poetry-2 in /home/elias/PycharmProjects/troubleshooting/poetry-2/.venv

Updating dependencies
Resolving dependencies... (1.1s)

Package operations: 38 installs, 0 updates, 0 removals

  - Installing setuptools (78.1.0)
  - Installing six (1.17.0)
  - Installing smmap (5.0.2)
  - Installing gitdb (4.0.12)
  - Installing charset-normalizer (3.4.1)
  - Installing certifi (2025.1.31)
  - Installing idna (3.10)
  - Installing packaging (24.2)
  - Installing ply (3.11)
  - Installing python-dateutil (2.9.0.post0)
  - Installing types-setuptools (78.1.0.20250329)
  - Installing tzdata (2025.2)
  - Installing urllib3 (2.4.0)
  - Installing wheel (0.45.1)
  - Installing gitpython (3.1.44)
  - Installing astunparse (1.6.3)
  - Installing hexbytes (1.3.0)
  - Installing giturlparse (0.12.0)
  - Installing orjson (3.10.16)
  - Installing makefun (1.15.6)
  - Installing pathvalidate (3.2.3)
  - Installing fsspec (2025.3.2)
  - Installing pendulum (3.0.0)
  - Installing jsonpath-ng (1.7.0)
  - Installing click (8.1.8)
  - Installing pluggy (1.5.0)
  - Installing humanize (4.12.2)
  - Installing psycopg2-binary (2.9.10)
  - Installing pytz (2025.2)
  - Installing pyyaml (6.0.2)
  - Installing requests (2.32.3)
  - Installing requirements-parser (0.11.0)
  - Installing semver (3.0.4)
  - Installing simplejson (3.20.1)
  - Installing tenacity (9.1.2)
  - Installing tomlkit (0.13.2)
  - Installing typing-extensions (4.13.2)
  - Installing dlt (1.3.0)

Writing lock file
```

</details>

<details>

<summary><b>Working:</b> Logs with uv pip install (uv version 0.6.14)</summary>

```console
PycharmProjects/troubleshooting/uv-3
❯ uv init --python 3.12
Initialized project `uv-3`

uv-3 on  main [?] is 📦 v0.1.0 via 🐍 v3.13.2
❯ uv venv
Using CPython 3.12.9 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

uv-3 on  main [?] is 📦 v0.1.0 via 🐍 v3.13.2
❯ uv pip install dlt[postgres]==1.3.0
Resolved 39 packages in 83ms
Installed 39 packages in 73ms
 + astunparse==1.6.3
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + click==8.1.8
 + dlt==1.3.0
 + fsspec==2025.3.2
 + gitdb==4.0.12
 + gitpython==3.1.44
 + giturlparse==0.12.0
 + hexbytes==1.3.0
 + humanize==4.12.2
 + idna==3.10
 + jsonpath-ng==1.7.0
 + makefun==1.15.6
 + orjson==3.10.16
 + packaging==24.2
 + pathvalidate==3.2.3
 + pendulum==3.0.0
 + pluggy==1.5.0
 + ply==3.11
 + psycopg2-binary==2.9.10
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + pyyaml==6.0.2
 + requests==2.32.3
 + requirements-parser==0.11.0
 + semver==3.0.4
 + setuptools==78.1.0
 + simplejson==3.20.1
 + six==1.17.0
 + smmap==5.0.2
 + tenacity==9.1.2
 + time-machine==2.16.0
 + tomlkit==0.13.2
 + types-setuptools==78.1.0.20250329
 + typing-extensions==4.13.2
 + tzdata==2025.2
 + urllib3==2.4.0
 + wheel==0.45.1
```
</details>

Let me know, if you guys need more information.

Best
Elias

### Platform

Linux 6.13.9-200.fc41.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.14

### Python version

Python 3.12.9,Python 3.13.2

---

_Label `bug` added by @trin94 on 2025-04-11 12:41_

---

_Comment by @ampaulo on 2025-05-23 10:37_

Hi,

If it may help someone, I've encountered this problem as well and installing the package `libpq-dev` solved the issue on my system:

OS version: **Ubuntu 24.04.2 LTS**
Python version: **3.12.9**
dlt version: **1.11.0**

`sudo apt install libpq-dev` 

`uv add dlt[postgres]>=1.11.0`


Paulo,



---

_Comment by @konstin on 2025-05-26 13:57_

`psycopg2cffi` is required for PyPy only. If you don't need PyPy, you can define `tool.uv.environments` to exclude exclude PyPy  (https://docs.astral.sh/uv/reference/settings/#environments):

```toml
[tool.uv]
environments = [
  "platform_python_implementation != 'PyPy'"
]
```

---

_Label `bug` removed by @konstin on 2025-05-26 13:57_

---

_Label `question` added by @konstin on 2025-05-26 13:57_

---

_Comment by @trin94 on 2025-05-27 19:01_

@konstin Yep, that makes a lot of sense. Did not know that since I've only used CPython up until now. That solves it for me. Thank you! :)

---

_Closed by @konstin on 2025-05-27 19:27_

---
