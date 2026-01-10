---
number: 9982
title: Does uv run support argcomplete?
type: issue
state: closed
author: carlpaten-ivadolabs
labels:
  - question
assignees: []
created_at: 2024-12-17T20:21:51Z
updated_at: 2025-06-12T08:37:45Z
url: https://github.com/astral-sh/uv/issues/9982
synced_at: 2026-01-10T01:24:48Z
---

# Does uv run support argcomplete?

---

_Issue opened by @carlpaten-ivadolabs on 2024-12-17 20:21_

I can't find in the documentation whether I can use `argcomplete` to get bash/zsh autocompletion when running `uv run myscript.py`.

This issue is two things:

* A documentation bug report
* A feature request, if uv doesn't have this already

---

_Comment by @zanieb on 2024-12-17 20:31_

Isn't `argcomplete` for `argparse`? Our argument parser isn't written in Python.

Please see https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion

---

_Label `question` added by @zanieb on 2024-12-17 20:31_

---

_Closed by @zanieb on 2025-01-06 20:15_

---

_Comment by @hoxell on 2025-04-14 08:36_

I believe @carlpaten was asking whether `uv` plugs into the mechanics of `argcomplete` when typing `uv run myscript.py <TAB>`, not `uv` itself, right?
(That's at least what I was looking for when I discovered this issue.)

---

_Comment by @ydirson on 2025-06-12 08:37_

Same here.

Example situation with `pytest`: with `python3-argcomplete` installed in my Debian 12, and `pytest` installed in a normal venv, the shell transparently provides completion for `pytest` (not sure of the exact mechanism, I guess it involves finding `$VENV/lib/python3.11/site-packages/_pytest/_argcomplete.py`).

Now when using `uv`:
- when using `uv run`, the completion does not seem to delegate command options to its completer
- when using `.venv` as a venv, `argcomplete` does not know about `pytest`'s completion, likely because of the differences with a real venv

---
