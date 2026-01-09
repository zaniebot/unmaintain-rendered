---
number: 2540
title: How can auto-completion be set up for Ruff?
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
  - question
assignees: []
created_at: 2023-02-03T13:50:20Z
updated_at: 2023-02-12T02:51:51Z
url: https://github.com/astral-sh/ruff/issues/2540
synced_at: 2026-01-07T13:12:14-06:00
---

# How can auto-completion be set up for Ruff?

---

_Issue opened by @JonathanPlasse on 2023-02-03 13:50_

I was wondering if there was a command to run to set up auto-completion for Ruff.
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-02-03 14:03_

We have `ruff generate-shell-completion [bash]` but no thorough docs.

---

_Label `documentation` added by @charliermarsh on 2023-02-03 14:03_

---

_Label `question` added by @charliermarsh on 2023-02-03 14:03_

---

_Comment by @JonathanPlasse on 2023-02-03 14:23_

We could take [poetry documentation](https://python-poetry.org/docs/#enable-tab-completion-for-bash-fish-or-zsh) as inspiration.

---

_Comment by @charliermarsh on 2023-02-03 14:27_

Yeah that looks good. We can add something to the README.

---

_Comment by @charliermarsh on 2023-02-03 16:34_

PR welcome if anyone wants to draft it :joy: Otherwise I'll get to it later.

---

_Comment by @JonathanPlasse on 2023-02-03 18:31_

I can work on it.

---

_Referenced in [astral-sh/ruff#2791](../../astral-sh/ruff/pulls/2791.md) on 2023-02-12 02:49_

---

_Closed by @charliermarsh on 2023-02-12 02:51_

---
