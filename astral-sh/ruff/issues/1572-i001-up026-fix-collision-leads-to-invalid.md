```yaml
number: 1572
title: I001 + UP026 fix collision leads to invalid trailing comma
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2023-01-02T23:12:26Z
updated_at: 2023-01-02T23:16:40Z
url: https://github.com/astral-sh/ruff/issues/1572
synced_at: 2026-01-12T15:54:41Z
```

# I001 + UP026 fix collision leads to invalid trailing comma

---

_@andersk_

You’ve probably noticed this from the failing CI, but here’s a minimal test case:

```python
if True:
    from mock import mock, a, b, c
```

After `ruff --select=I001,UP026`:

```python
if True:
    from unittest.mock import a, b, c, 
```

which has an invalid trailing comma.

---

_Comment by @andersk on 2023-01-02 23:13_

Oh yes, you have (#1570).

---

_Closed by @andersk on 2023-01-02 23:13_

---

_Comment by @charliermarsh on 2023-01-02 23:16_

😅 

---
