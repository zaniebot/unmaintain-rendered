---
number: 10082
title: Ruff Format does not remove redundant parentheses
type: issue
state: closed
author: Tialo
labels:
  - formatter
  - style
assignees: []
created_at: 2024-02-22T16:13:29Z
updated_at: 2024-02-22T16:18:50Z
url: https://github.com/astral-sh/ruff/issues/10082
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff Format does not remove redundant parentheses

---

_Issue opened by @Tialo on 2024-02-22 16:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
code to reproduce:
```python
def x():
    return ((1,), 2)
```
expected to be reformatted to:
```python
def x():
    return (1,), 2
```
ruff 0.2.2

i used
`ruff format --isolated file.py`

---

_Comment by @MichaReiser on 2024-02-22 16:18_

I understand the desire for parentheses normalization. I'm very used to that behavior from using Prettier. 

However, it\s something neither black nor Ruff do today because it can change your program's AST representation and semantics (not in this case, but with expressions). That's why both formatter tend to preserve parentheses except in clause headers. 

We may want to change this more holistically in the future, but it isn't something we plan to change soon. 

Aside from the normalization: Keeping the tuple parentheses can help to make it clear that the returned value is a tuple.


---

_Closed by @MichaReiser on 2024-02-22 16:18_

---

_Label `formatter` added by @MichaReiser on 2024-02-22 16:18_

---

_Label `style` added by @MichaReiser on 2024-02-22 16:18_

---
