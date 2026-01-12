```yaml
number: 739
title: Pytest xfail
type: issue
state: closed
author: Kumzy
labels: []
assignees: []
created_at: 2025-07-01T13:09:01Z
updated_at: 2025-07-01T13:27:32Z
url: https://github.com/astral-sh/ty/issues/739
synced_at: 2026-01-12T15:54:23Z
```

# Pytest xfail

---

_@Kumzy_

### Summary

The call of xfail function from pytest generate an error

https://docs.pytest.org/en/stable/how-to/skipping.html#xfail-mark-test-functions-as-expected-to-fail

```python
import pytest

def test_function2():
    pytest.xfail("slow_module taking too long")
```

Running the following
```
uvx ty check pytest.py
```

```sh
error[call-non-callable]: Object of type `_WithException[Unknown, <class 'XFailed'>]` is not callable
 --> ty/pytest.py:4:5
  |
3 | def test_function2():
4 |     pytest.xfail("slow_module taking too long")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `call-non-callable` is enabled by default
```

### Version

_No response_

---

_Closed by @AlexWaygood on 2025-07-01 13:11_

---

_Comment by @AlexWaygood on 2025-07-01 13:11_

Thanks! This is the same underlying issue as #553

---

_Comment by @Kumzy on 2025-07-01 13:26_

Oh I searched for **xfail** and did not find anything, should have used different terms. Sorry

---

_Comment by @AlexWaygood on 2025-07-01 13:27_

no worries, GitHub issue search is terrible :-)

---
