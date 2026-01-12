```yaml
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
synced_at: 2026-01-12T15:54:43Z
```

# ruff and black disagree on `.pyi` format for one line `def`

---

_@messense_

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

_Closed by @charliermarsh on 2023-02-11 17:34_

---
