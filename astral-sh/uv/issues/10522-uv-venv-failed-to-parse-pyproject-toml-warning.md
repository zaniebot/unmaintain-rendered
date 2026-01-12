```yaml
number: 10522
title: "uv venv \"Failed to parse: `pyproject.toml`\" warning without the root cause + why the project version in `pyproject.toml`"
type: issue
state: closed
author: pabepadu
labels:
  - error messages
assignees: []
created_at: 2025-01-11T20:10:27Z
updated_at: 2025-01-13T19:06:33Z
url: https://github.com/astral-sh/uv/issues/10522
synced_at: 2026-01-12T16:00:15Z
```

# uv venv "Failed to parse: `pyproject.toml`" warning without the root cause + why the project version in `pyproject.toml`

---

_@pabepadu_

Hi, first uv is a really great tool, thanks!

I'm using version 0.5.18 and I found two issues with this `pyproject.toml`:
```toml
[project]
name = "dummy-name"
requires-python = ">=3.12, <3.13"
```

1. `uv venv` returns no explanation why as to why it could not parse the `pyproject.toml` (same with verbose mode):
    ```bash
    $ uv venv
    warning: Failed to parse: `pyproject.toml`
    Using CPython 3.13.1
    Creating virtual environment at: .venv
    Activate with: source .venv/Scripts/activate
    ```
    And in this case the wrong version of python is selected.

    By luck I found the root cause with another command:
    ```bash
    $ uv tree
    error: Failed to parse: `pyproject.toml`
      Caused by: TOML parse error at line 1, column 1
      |
    1 | [project]
      | ^^^^^^^^^^
    `pyproject.toml` is using the `[project]` table, but the required `project.version` field is neither set nor present in the `project.dynamic` list
    ```

    Is it possible to improve the warning message with the root cause or change the warning to an error like in `uv tree` ?

2. Why a project `version` is mandatory to be able to parse the `pyproject.toml` ?
    In this case I don't expect to publish the project, so the version is meaningless

Thanks in advance

---

_Comment by @charliermarsh on 2025-01-11 20:12_

`uv venv` doesn't require the Python version, but it does try to parse `pyproject.toml` in case you have uv configuration in there. So the warning you see (I think( is uv failing to parse the `pyproject.toml` while trying to extract any configuration. If we fail, we just warn, rather than failing hard, since it's not _required_ for that command.

I'm surprised we don't show the full error, though.

---

_Label `error messages` added by @charliermarsh on 2025-01-11 20:12_

---

_Comment by @charliermarsh on 2025-01-11 20:12_

(From my perspective, we _should_ show that error in this case, but it should be the expanded version.)

---

_Comment by @zanieb on 2025-01-11 20:40_

Regarding

> Why a project version is mandatory to be able to parse the pyproject.toml ?

If you have a `[project]` table, the `version` field is mandatory per the specification https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#version

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-13 02:47_

---

_Closed by @charliermarsh on 2025-01-13 19:06_

---

_Closed by @charliermarsh on 2025-01-13 19:06_

---
