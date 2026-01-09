---
number: 2763
title: "ruff and black disagree on `.pyi` format for one line `def`"
type: issue
state: closed
author: messense
labels:
  - bug
assignees: []
created_at: 2023-02-11T13:04:49Z
updated_at: 2023-02-11T17:34:22Z
url: https://github.com/astral-sh/ruff/issues/2763
synced_at: 2026-01-07T13:12:14-06:00
---

# ruff and black disagree on `.pyi` format for one line `def`

---

_Issue opened by @messense on 2023-02-11 13:04_

```python
class DummyClass:
    @staticmethod
    def get_42() -> int: ...
```

ruff gives `test-crates/pyo3-pure/pyo3_pure.pyi:3:5: E704 Multiple statements on one line (def)` while black formats it back to one line if you change it to

```python
class DummyClass:
    @staticmethod
    def get_42() -> int:
        ...
```

---

_Renamed from "ruff and black disagree on `.pyi` format" to "ruff and black disagree on `.pyi` format for one line `def`" by @messense on 2023-02-11 13:05_

---

_Label `bug` added by @charliermarsh on 2023-02-11 17:01_

---

_Comment by @charliermarsh on 2023-02-11 17:01_

Need to figure out how to handle this because Flake8 and pycodestyle actually _do_ flag these as errors (they just don't enable that rule by default, AFAICT).

---

_Comment by @charliermarsh on 2023-02-11 17:29_

I'm going to either disable this rule by default or remove it entirely.

---

_Referenced in [astral-sh/ruff#2773](../../astral-sh/ruff/pulls/2773.md) on 2023-02-11 17:33_

---

_Closed by @charliermarsh on 2023-02-11 17:34_

---

_Referenced in [astral-sh/ruff#2402](../../astral-sh/ruff/issues/2402.md) on 2025-04-07 14:29_

---
