---
number: 9319
title: "Implement new SIM rule SIM911: `zip(dict.keys(), dict.values()) → dict.items()`"
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - rule
assignees: []
created_at: 2023-12-30T17:43:12Z
updated_at: 2024-01-11T19:42:44Z
url: https://github.com/astral-sh/ruff/issues/9319
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement new SIM rule SIM911: `zip(dict.keys(), dict.values()) → dict.items()`

---

_Issue opened by @Skylion007 on 2023-12-30 17:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Flake8 simplify was updated recently and a simple, but effective new rule was added.
```python
a = dict(...)
items = zip(a.keys(), a.values())
```

should instead be
```python
items = a.items()
```

@dosisod Refurb might be interested in this rule as well.



---

_Label `rule` added by @charliermarsh on 2023-12-31 12:40_

---

_Label `accepted` added by @charliermarsh on 2023-12-31 12:40_

---

_Label `good first issue` added by @charliermarsh on 2023-12-31 12:40_

---

_Label `accepted` removed by @charliermarsh on 2023-12-31 12:40_

---

_Comment by @trag1c on 2024-01-03 19:10_

I'd like to work on this, please assign me :)

---

_Assigned to @trag1c by @charliermarsh on 2024-01-03 19:11_

---

_Referenced in [astral-sh/ruff#9460](../../astral-sh/ruff/pulls/9460.md) on 2024-01-10 23:14_

---

_Closed by @charliermarsh on 2024-01-11 19:42_

---
