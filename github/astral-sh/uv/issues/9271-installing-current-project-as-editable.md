---
number: 9271
title: installing current project as editable
type: issue
state: closed
author: zkurtz
labels:
  - question
assignees: []
created_at: 2024-11-20T11:00:47Z
updated_at: 2025-03-13T15:40:10Z
url: https://github.com/astral-sh/uv/issues/9271
synced_at: 2026-01-07T13:12:18-06:00
---

# installing current project as editable

---

_Issue opened by @zkurtz on 2024-11-20 11:00_

Is there a single uv command that achieves both `uv sync; uv pip install -e .`? 

In my project's young and hackish workflow, the convention is to activate the virtual env and then cd into other directories to run stuff (still using the same venv). In that case, I need my project to be installed (as editable) in its own venv. The instructions [here](https://docs.astral.sh/uv/pip/packages/#editable-packages) say to do `uv pip install -e .`. But this makes no changes to the `uv.lock`, and correspondingly `uv sync` undoes the initial action. Which means that users need to remember to run `uv pip install -e .` after every time they do `uv sync`.

I noticed there is also the option of doing `uv add myproject --dev`, which looks like it _could_ be the right idea, but it doesn't seem to actually install anything, as `import myproject` does not work from other directories (with the same venv activated).


---

_Comment by @konstin on 2024-11-20 11:10_

Does your project declare a build backend? `uv sync` only editably installs projects that declare one explicitly.

---

_Label `question` added by @charliermarsh on 2024-11-20 13:30_

---

_Comment by @zkurtz on 2024-11-20 13:49_

Thank you! Indeed adding this to my pyproject.toml resolved the issue, now `uv sync` behaves as desired:
```
[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```
However, I noticed that `uv build` assumes a default backend in case on is not provided. I wonder if `uv sync` could be modified to make a similar assumption?

---

_Comment by @konstin on 2024-11-20 13:56_

This is a tradeoff we're making: We want to support people who don't write a package but only have scripts that shouldn't be installed (https://docs.astral.sh/uv/concepts/projects/#build-systems), while in `uv build`, we have to assume a setuptools default for backwards compatibility (PEP 517).

---

_Closed by @zkurtz on 2024-11-20 18:42_

---

_Comment by @KantiCodes on 2025-03-13 15:40_

> Does your project declare a build backend? `uv sync` only editably installs projects that declare one explicitly.

Thank you so much!

---
