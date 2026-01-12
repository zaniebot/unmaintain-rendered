```yaml
number: 2207
title: "Type narrowing doesn't work with `Never` functions"
type: issue
state: closed
author: albireox
labels: []
assignees: []
created_at: 2025-12-24T16:25:08Z
updated_at: 2025-12-24T16:31:24Z
url: https://github.com/astral-sh/ty/issues/2207
synced_at: 2026-01-12T15:54:26Z
```

# Type narrowing doesn't work with `Never` functions

---

_@albireox_

### Summary

Type narrowing doesn't seem to work with a function that returns `Never` (in this case because it raises an error). For example,

```python
# ln2_temps is int | float | None
if ln2_temp is None:
    # self.fail raises and error and it's annotated are returning Never
    self.fail(f"Failed retrieving {camera!r} temperature.")

# Here ty reports an error Unsupported `>` operation because it thinks ln2_temps can still be None
if ln2_temp > max_temperature:
    ...
```

Here is a Playground example: https://play.ty.dev/858f2449-f375-4deb-bb69-94485497b215

### Version

0.0.6

---

_Comment by @AlexWaygood on 2025-12-24 16:29_

Thanks, please see #690

---

_Closed by @AlexWaygood on 2025-12-24 16:29_

---

_Comment by @albireox on 2025-12-24 16:31_

Ah, didn't find that one. Seem to be the same issue.

---
