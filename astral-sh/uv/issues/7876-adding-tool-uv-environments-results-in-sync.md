```yaml
number: 7876
title: "adding tool.uv.environments results in `sync --locked` always failing"
type: issue
state: closed
author: garymm
labels:
  - bug
assignees: []
created_at: 2024-10-02T17:59:56Z
updated_at: 2024-10-03T13:15:09Z
url: https://github.com/astral-sh/uv/issues/7876
synced_at: 2026-01-12T15:59:17Z
```

# adding tool.uv.environments results in `sync --locked` always failing

---

_@garymm_

Hi. Thank you for uv! It's awesome.
I'm not sure if I'm doing something wrong, but I can't get `sync --locked` to work when specifying `tool.uv.environments` in my pyproject.toml. Example:
```
#:schema https://json.schemastore.org/pyproject.json
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project]
name = "demo"
requires-python = ">=3.11,<3.12"
# https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#static-vs-dynamic-metadata
dynamic = ["version"]
dependencies = [
    "pytest>=8.3.3",
]

[tool.uv]
environments = [
    "sys_platform == 'darwin' and platform_machine == 'arm64' and python_version>='3.11' and python_version<'3.12'",
    "sys_platform == 'linux' and platform_machine == 'aarch64' and python_version>='3.11' and python_version<'3.12'",
    "sys_platform == 'linux' and platform_machine == 'x86_64' and python_version>='3.11' and python_version<'3.12'",
]
```

```
➜ uv --version
uv 0.4.17
➜ uv lock
Using CPython 3.11.9
Resolved 5 packages in 682ms
➜ uv sync --locked
Using CPython 3.11.9
Creating virtual environment at: .venv
Resolved 5 packages in 4ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

---

_Comment by @zanieb on 2024-10-02 18:07_

Weird, thanks.

cc @BurntSushi / @charliermarsh 

---

_Label `bug` added by @zanieb on 2024-10-02 18:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-02 22:59_

---

_Comment by @charliermarsh on 2024-10-02 23:11_

Thanks, I'll fix this tomorrow!

---

_Comment by @charliermarsh on 2024-10-02 23:17_

(I see the issue.)

---

_Closed by @charliermarsh on 2024-10-03 13:15_

---

_Closed by @charliermarsh on 2024-10-03 13:15_

---
