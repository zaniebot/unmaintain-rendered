```yaml
number: 20632
title: C413 documentation implies the fix is unsafer than it is
type: issue
state: closed
author: dscorbett
labels:
  - documentation
assignees: []
created_at: 2025-09-29T16:58:25Z
updated_at: 2025-10-03T14:23:58Z
url: https://github.com/astral-sh/ruff/issues/20632
synced_at: 2026-01-10T11:09:59Z
```

# C413 documentation implies the fix is unsafer than it is

---

_Issue opened by @dscorbett on 2025-09-29 16:58_

### Summary

The documentation for [`unnecessary-call-around-sorted` (C413)](https://docs.astral.sh/ruff/rules/unnecessary-call-around-sorted/#fix-safety) says:
> This rule's fix is marked as unsafe, as `reversed()` and `reverse=True` will yield different results in the event of custom sort keys or equality functions. Specifically, `reversed()` will reverse the order of the collection, while `sorted()` with `reverse=True` will perform a stable reverse sort, which will preserve the order of elements that compare as equal.

That makes it sound like the fix is always marked as unsafe. In fact, it is marked as unsafe for `reversed` and safe for `list`.

---

_Label `documentation` added by @AlexWaygood on 2025-09-29 17:09_

---

_Closed by @dylwil3 on 2025-10-03 14:23_

---
