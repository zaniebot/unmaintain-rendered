```yaml
number: 2455
title: "`not hasattr(...)` causes conditional block to resolve `self` to `Never`"
type: issue
state: open
author: moschi
labels:
  - narrowing
assignees: []
created_at: 2026-01-12T08:08:37Z
updated_at: 2026-01-13T06:31:17Z
url: https://github.com/astral-sh/ty/issues/2455
synced_at: 2026-01-13T07:25:22Z
```

# `not hasattr(...)` causes conditional block to resolve `self` to `Never`

---

_@moschi_

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

_Comment by @carljm on 2026-01-13 02:08_

Thanks for the report!

Given that we don't (currently) distinguish between maybe-bound and definitely-bound attributes, I guess we can't safely do negative narrowing on `hasattr`, only positive.

---

_Added to milestone `Stable` by @carljm on 2026-01-13 02:08_

---

_Renamed from "not hasattr(...) causes conditional block to resolve self to Never" to "`not hasattr(...)` causes conditional block to resolve `self` to `Never`" by @moschi on 2026-01-13 06:31_

---
