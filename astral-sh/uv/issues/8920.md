```yaml
number: 8920
title: Purpose of .python-version?
type: issue
state: closed
author: denkasyanov
labels: []
assignees: []
created_at: 2024-11-08T05:48:24Z
updated_at: 2024-11-08T23:14:37Z
url: https://github.com/astral-sh/uv/issues/8920
synced_at: 2026-01-10T04:36:20Z
```

# Purpose of .python-version?

---

_Issue opened by @denkasyanov on 2024-11-08 05:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Currently on `uv init` a project boilerplate created includes the file `.python-version`.

What is the purpose of this file, if there are constraints in `pyproject.toml` and `uv.lock`?

Is it just for compatibility with tools like pyenv or is there more to it?

I really like how clean project directory became after moving a lot of stuff (like `requirements.*`) to `pyproject.toml`. So wonder, if `.python-version` is really required in two senses:
1. Is it required for the projects now? I tested by deleting `.python-version`, and everything works as expected and this file is not recreated with commands like `uv run`, `uv sync` and `uv lock`
2. Since a lot of constraints/pinned versions of things are now in `pyproject.toml` and `uv.lock`, maybe if `.python-version` still plays some important role, its function can be moved to `pyproject.toml` or `uv.lock` in future?

Asking as a 5-year user of `pyenv` and `.python-version`. They are great, but I don't really miss them.

Also, couldn't find peps related to this file. 

---

_Comment by @my1e5 on 2024-11-08 15:13_

Related - https://github.com/astral-sh/uv/issues/8247

A `.python-version` file is not strictly required, but it's useful to have it when developing a project as it allows you to specify the exact Python version you are using to do development. It's different to the `requires-python` field  - that is the range of Python versions supported by your project.



---

_Comment by @denkasyanov on 2024-11-08 23:14_

Thanks!

It matches my intuition and #8247 indeed provides useful context!

---

_Closed by @denkasyanov on 2024-11-08 23:14_

---
