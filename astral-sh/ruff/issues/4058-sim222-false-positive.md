```yaml
number: 4058
title: SIM222 False positive
type: issue
state: closed
author: martinhoyer
labels:
  - bug
assignees: []
created_at: 2023-04-21T11:45:28Z
updated_at: 2023-04-25T04:44:04Z
url: https://github.com/astral-sh/ruff/issues/4058
synced_at: 2026-01-12T15:54:44Z
```

# SIM222 False positive

---

_@martinhoyer_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Related to https://github.com/MartinThoma/flake8-simplify/issues/142 ?
Similar to #3830

```
something = 'foo' or True
```
```
$ ruff --show-source sim222.py 
sim222.py:1:13: SIM222 [*] Use `True` instead of `... or True`
  |
1 | something = 'foo' or True
  |             ^^^^^^^^^^^^^ SIM222
  |
  = help: Replace with `True`
```

---

_Comment by @dhruvmanila on 2023-04-21 15:51_

This might be easier to fix if we can detect whether a node is _truthy_ or _falsey_. The `__bool__` method on an object adds to this complexity as a custom object might be interested in a different behavior. I guess this boils down to type inference.

---

_Comment by @JonathanPlasse on 2023-04-22 09:56_

I would like to work on it.

---

_Label `bug` added by @charliermarsh on 2023-04-25 00:13_

---

_Assigned to @JonathanPlasse by @charliermarsh on 2023-04-25 00:13_

---

_Closed by @charliermarsh on 2023-04-25 04:44_

---
