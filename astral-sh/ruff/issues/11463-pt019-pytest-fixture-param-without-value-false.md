```yaml
number: 11463
title: "`PT019` `pytest-fixture-param-without-value` False positive when using patch"
type: issue
state: closed
author: augustelalande
labels:
  - bug
assignees: []
created_at: 2024-05-18T21:06:07Z
updated_at: 2025-05-15T13:34:41Z
url: https://github.com/astral-sh/ruff/issues/11463
synced_at: 2026-01-12T15:54:51Z
```

# `PT019` `pytest-fixture-param-without-value` False positive when using patch

---

_@augustelalande_

This presumably applies to any test function with an unused argument, which is not actually a test fixture. But I'm specifically in the situation bellow.
```python
from unittest.mock import Mock, patch

@patch("package.something")
def test_something(_mock: Mock):
    ...
```
I am using pytest, but also using unittest.mock.patch to modify the behaviour of my test target. The patch decorator returns the mocked object as an argument so I have to accept it, even though I don't use it. PT019 then assumes I'm receiving an unused fixture, and suggests a fix that doesn't work.

---

_Label `bug` added by @charliermarsh on 2024-05-19 03:39_

---

_Closed by @MichaReiser on 2025-05-15 13:34_

---
