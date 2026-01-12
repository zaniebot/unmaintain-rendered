```yaml
number: 3957
title: "Add ability to disable D102 only for methods with an @property decorator"
type: issue
state: open
author: Matt-Ord
labels:
  - docstring
assignees: []
created_at: 2023-04-13T14:06:52Z
updated_at: 2023-04-13T17:25:23Z
url: https://github.com/astral-sh/ruff/issues/3957
synced_at: 2026-01-12T15:54:44Z
```

# Add ability to disable D102 only for methods with an @property decorator

---

_@Matt-Ord_

I'm not sure if it is possible to disable D102 for property getters without also disabling the warning for non property methods

```python
class Test:
    # Missing docstring in public method Ruff(D102)
    def test_method(self):
        pass

    # Should be able to disable the warning here only
    # Missing docstring in public method Ruff(D102)
    @property
    def test_property(self):
        pass
```

---

_Comment by @charliermarsh on 2023-04-13 17:25_

I don't think this is possible right now. We ignore setters and deleters, but not getters, and I don't think its configurable.

---

_Label `docstring` added by @charliermarsh on 2023-04-13 17:25_

---
