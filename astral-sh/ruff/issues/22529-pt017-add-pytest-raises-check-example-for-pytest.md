```yaml
number: 22529
title: "PT017: add `pytest.raises(..., check=...)` example (for pytest ≥ 8.4.0)"
type: issue
state: closed
author: harupy
labels:
  - documentation
assignees: []
created_at: 2026-01-12T09:17:38Z
updated_at: 2026-01-13T20:45:54Z
url: https://github.com/astral-sh/ruff/issues/22529
synced_at: 2026-01-13T21:36:17Z
```

# PT017: add `pytest.raises(..., check=...)` example (for pytest ≥ 8.4.0)

---

_@harupy_

The [PT017](https://docs.astral.sh/ruff/rules/pytest-assert-in-except/#pytest-assert-in-except-pt017) documentation currently shows validating exception details by binding `exc_info` from `pytest.raises` and asserting on it afterward.

Pytest ≥ 8.4.0 supports a `check` parameter on `pytest.raises`, which allows performing this validation directly inside the context manager: https://docs.pytest.org/en/stable/reference/reference.html#pytest-raises


```python
import pytest


def test_foo():
    # Current PT017 example:
    with pytest.raises(ZeroDivisionError) as exc_info:
        1 / 0
    assert exc_info.value.args


def test_bar():
    # Proposed
    with pytest.raises(ZeroDivisionError, check=lambda e: e.args):
        1 / 0
```


### Version

_No response_

---

_Label `question` added by @harupy on 2026-01-12 09:17_

---

_Renamed from "PT017 docs: add `pytest.raises(..., check=...)` example (pytest ≥ 8.4.0)" to "PT017: add `pytest.raises(..., check=...)` example (pytest ≥ 8.4.0)" by @harupy on 2026-01-12 09:17_

---

_Comment by @harupy on 2026-01-12 09:18_

Sorry, this is not a question but a rule request. If this request makes sense, I can file a PR.

---

_Renamed from "PT017: add `pytest.raises(..., check=...)` example (pytest ≥ 8.4.0)" to "PT017: add `pytest.raises(..., check=...)` example (for pytest ≥ 8.4.0)" by @harupy on 2026-01-12 09:49_

---

_Comment by @ntBre on 2026-01-12 15:00_

Makes sense to me! This also seems related to the changes in https://github.com/astral-sh/ruff/pull/21964.

---

_Label `question` removed by @ntBre on 2026-01-12 15:00_

---

_Label `documentation` added by @ntBre on 2026-01-12 15:00_

---

_Closed by @ntBre on 2026-01-13 20:45_

---
