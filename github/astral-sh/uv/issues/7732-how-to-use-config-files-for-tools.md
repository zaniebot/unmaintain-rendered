---
number: 7732
title: How to use config files for tools?
type: issue
state: closed
author: andreas-vester
labels:
  - question
assignees: []
created_at: 2024-09-27T07:28:00Z
updated_at: 2024-12-02T17:41:58Z
url: https://github.com/astral-sh/uv/issues/7732
synced_at: 2026-01-07T13:12:17-06:00
---

# How to use config files for tools?

---

_Issue opened by @andreas-vester on 2024-09-27 07:28_

When using tools like ``ruff``, ``mypy``, ``pytest`` etc., I am using config files, such as ``ruff.toml`` or dedicated sections in ``pyproject.toml`` file.

If installing these tools via ``uv tool install``, where am I supposed to put the respective configurations?

Also, I need to make sure that all team members do have the same settings, which is probably not as easy as putting them into a concrete project repo, isn't it?

---

_Comment by @charliermarsh on 2024-09-27 12:27_

It depends on the tool. Ruff, for example, supports user-level configuration: https://docs.astral.sh/ruff/faq/#how-can-i-change-ruffs-default-configuration. So you'd put it at `~/.config/ruff/ruff.toml`. Or, you can put a `ruff.toml` at the root of each project (or anywhere on the filesystem really) and Ruff will discover it when checking those files.

MyPy almost always needs to be installed into the virtual environment that it's type-checking, so it generally doesn't make sense to install as a tool.

(I'm not certain on how `pytest` configuration works, but again, it probably makes sense for that to be a project dev dependency rather than a tool.)

---

_Label `question` added by @charliermarsh on 2024-09-27 12:27_

---

_Closed by @andreas-vester on 2024-09-28 13:36_

---

_Comment by @asmaier on 2024-12-02 17:41_

For people who end up here I want to give an example. If you want to configure `ruff` you can just add some dedicated sections `[tool.ruff*]` to your `pyproject.toml` created by `uv` like so

```
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "anonymous", email = "anonymous@example.com" }
]
requires-python = ">=3.11"
dependencies = [
    "python-dotenv>=1.0.1",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[dependency-groups]
dev = [
    "pytest>=8.3.4",
    "ruff>=0.8.1",
]

[tool.ruff]
target-version = "py311"
line-length = 120

[tool.ruff.lint]
extend-select = ["E", "F"]
```

Then you can just run `ruff`

    uv run ruff check

and it will pick up your configuration from the `pyproject.toml`.

See also
- https://docs.astral.sh/ruff/configuration/




---
