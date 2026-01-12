```yaml
number: 22158
title: "Rule idea: Prefer `monkeypatch.setenv` over direct `os.environ` mutation in tests"
type: issue
state: open
author: harupy
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-23T09:46:23Z
updated_at: 2025-12-23T14:20:18Z
url: https://github.com/astral-sh/ruff/issues/22158
synced_at: 2026-01-12T15:54:58Z
```

# Rule idea: Prefer `monkeypatch.setenv` over direct `os.environ` mutation in tests

---

_@harupy_

### Summary

Directly mutating `os.environ` in tests can leak state across test cases and cause order-dependent failures. Use pytest’s `monkeypatch.setenv`, which automatically restores environment variables after the test completes, ensuring proper isolation and reproducibility.

### Example

```python
import os
import pytest

def foo() -> str:
    return os.environ.get("ENV_VAR", "default_value")


# Bad
def test_bad():
    os.environ["ENV_VAR"] = "value"
    assert foo() == "value"


# Good
def test_good(monkeypatch: pytest.MonkeyPatch):
    monkeypatch.setenv("ENV_VAR", "value")
    assert foo() == "value"
```

---

_Renamed from "Prefer `monkeypatch.setenv` over direct `os.environ` mutation in tests" to "Rule idea: Prefer `monkeypatch.setenv` over direct `os.environ` mutation in tests" by @harupy on 2025-12-23 13:53_

---

_Label `rule` added by @ntBre on 2025-12-23 14:20_

---

_Label `needs-decision` added by @ntBre on 2025-12-23 14:20_

---
