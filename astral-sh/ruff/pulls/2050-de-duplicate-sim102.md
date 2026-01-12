```yaml
number: 2050
title: De-duplicate SIM102
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: de-duplicate-SIM102
created_at: 2023-01-21T04:15:51Z
updated_at: 2023-01-21T04:38:53Z
url: https://github.com/astral-sh/ruff/pull/2050
synced_at: 2026-01-12T15:55:07Z
```

# De-duplicate SIM102

---

_@harupy_

The idea is the same as #1867. Avoids emitting `SIM102` twice for the following code:

```python
if a:
    if b:
        if c:
            d
```

```
resources/test/fixtures/flake8_simplify/SIM102.py:1:1: SIM102 Use a single `if` statement instead of nested `if` statements
resources/test/fixtures/flake8_simplify/SIM102.py:2:5: SIM102 Use a single `if` statement instead of nested `if` statements
```

---

_Merged by @charliermarsh on 2023-01-21 04:38_

---

_Closed by @charliermarsh on 2023-01-21 04:38_

---
