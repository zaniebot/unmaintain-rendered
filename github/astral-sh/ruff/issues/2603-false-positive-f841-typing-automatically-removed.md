---
number: 2603
title: "False positive F841: typing automatically removed as unused by ``--fix`` when they are not"
type: issue
state: closed
author: Pierre-Sassoulas
labels:
  - bug
  - question
assignees: []
created_at: 2023-02-06T10:23:50Z
updated_at: 2023-02-06T20:45:25Z
url: https://github.com/astral-sh/ruff/issues/2603
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive F841: typing automatically removed as unused by ``--fix`` when they are not

---

_Issue opened by @Pierre-Sassoulas on 2023-02-06 10:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I'm using v0.241, on the following code:

```python
KeyTupleT = Tuple[Type[nodes.NodeNG], str]
keys_checked: set[KeyTupleT] = set()
```
The following conf:
```toml
[tool.ruff]
select = ["E", "F"]
fixable = ["A", "B", "C", "D", "E", "F"]
```
The output of ``ruff --fix`` is:
```python
Tuple[Type[nodes.NodeNG], str]
keys_checked: set[KeyTupleT] = set()
```
But ``KeyTupleT`` is used in the typing.

---

_Renamed from "False positive: typing automatically removed as unused by ``--fix`` when they are not" to "False positive F841: typing automatically removed as unused by ``--fix`` when they are not" by @Pierre-Sassoulas on 2023-02-06 10:25_

---

_Label `bug` added by @charliermarsh on 2023-02-06 15:35_

---

_Comment by @charliermarsh on 2023-02-06 15:35_

Definitely looks like a bug -- will take a look!

---

_Comment by @charliermarsh on 2023-02-06 15:57_

I'm unable to reproduce this. Is that the full code snippet and configuration? Are you able to repro with `--isolated` at all?

I wonder if you perhaps have a lower Python version set, and so we're not treating `set[X]` as a reference to `X`? Just a guess...

---

_Label `question` added by @charliermarsh on 2023-02-06 15:57_

---

_Comment by @Pierre-Sassoulas on 2023-02-06 18:49_

I just tried but I can't reproduce using only ruff either. The issue happen using ``pre-commit`` and the conf in our ``pyproject.toml``.
```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: "v0.0.241"
    hooks:
      - id: ruff
        args: ["--fix"]
        exclude: *fixtures
```        
```toml
[tool.ruff]
select = ["E", "F", "B"]
ignore = [
    # "F841", # False positive for typing
    "B905", # Not enforced previously
]
fixable = ["A", "B", "C", "D", "E", "F"]
line-length = 125
```
 Strangely I do not manage to reproduce with the minimal example like I originally did... I don't know exactly why, I added some configuration for ``B`` message and ignore since. But I'm still reproducing the original bug in https://github.com/Pierre-Sassoulas/pylint/tree/add-ruff-to-pre-commit (If you remove ``"F841", # False positive for typing`` in ``pyproject.toml`` and launch ``pre-commit run ruff --all-files``), there's a modification in ``pylint/extensions/code_style.py`` for both python 3.10 and python 3.11.

---

_Comment by @charliermarsh on 2023-02-06 18:53_

Okay thanks, let me take another look...

---

_Referenced in [pylint-dev/pylint#8197](../../pylint-dev/pylint/pulls/8197.md) on 2023-02-06 18:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-06 19:04_

---

_Comment by @charliermarsh on 2023-02-06 19:34_

Only seeing this with `from __future__ import annotations`.

---

_Referenced in [astral-sh/ruff#2607](../../astral-sh/ruff/pulls/2607.md) on 2023-02-06 19:37_

---

_Comment by @charliermarsh on 2023-02-06 19:40_

Fixed, this'll be out in today's release.

---

_Closed by @charliermarsh on 2023-02-06 19:40_

---

_Comment by @Pierre-Sassoulas on 2023-02-06 20:44_

The bug fixing is as fast as the tool itself 😄 ⚡ 

---

_Comment by @charliermarsh on 2023-02-06 20:45_

Thanks for filing :) Surprised we haven't run into this one before! I guess locally defined type definitions aren't _that_ common.

---
