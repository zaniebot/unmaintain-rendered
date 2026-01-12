```yaml
number: 2455
title: not hasattr(...) causes conditional block to resolve self to Never
type: issue
state: open
author: moschi
labels:
  - narrowing
assignees: []
created_at: 2026-01-12T08:08:37Z
updated_at: 2026-01-12T08:12:32Z
url: https://github.com/astral-sh/ty/issues/2455
synced_at: 2026-01-12T08:52:44Z
```

# not hasattr(...) causes conditional block to resolve self to Never

---

_Issue opened by @moschi on 2026-01-12 08:08_

### Summary

The following code snipped illustrates the issue:

```python
class NeverWithHasAttr:
    @property
    def property_a(self) -> str:
        if not hasattr(self, "_property_a"):
            self._property_a = f"{self.this_does_not_exist} does not cause an error, self is Never"
        return self._property_a

    @property
    def property_b(self) -> str:
        if getattr(self, "_property_b", None) is None:
            self._property_b = f"{self.this_does_not_exist} causes an error"
        return self._property_b

```

where `self` inside the `not hasattr(self, "_property_a"):` block is Never, thus not causing any misspelled properties to be flagged as errors.

### Version

ty 0.0.11

---

_Label `narrowing` added by @AlexWaygood on 2026-01-12 08:11_

---
