```yaml
number: 1825
title: Correctly handle variadic parameters for overload step 5 filtering
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - overloads
assignees: []
created_at: 2025-12-09T14:56:40Z
updated_at: 2025-12-10T16:53:58Z
url: https://github.com/astral-sh/ty/issues/1825
synced_at: 2026-01-12T15:54:25Z
```

# Correctly handle variadic parameters for overload step 5 filtering

---

_@dhruvmanila_

### Summary

https://github.com/astral-sh/ruff/pull/21859 doesn't correctly fix the underlying issue and there are multiple TODOs in the test: https://github.com/astral-sh/ruff/blob/b21d520f067f1add859bf6494e28a3c5c7a020f4/crates/ty_python_semantic/resources/mdtest/call/overloads.md#varidic-argument-with-generics that needs to be resolved.

### Version

_No response_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-09 14:56_

---

_Label `overloads` added by @dhruvmanila on 2025-12-09 14:56_

---

_Label `bug` added by @dhruvmanila on 2025-12-09 14:56_

---

_Added to milestone `Beta` by @dhruvmanila on 2025-12-09 15:00_

---

_Removed from milestone `Beta` by @dhruvmanila on 2025-12-10 16:53_

---

_Added to milestone `Stable` by @dhruvmanila on 2025-12-10 16:53_

---

_Comment by @dhruvmanila on 2025-12-10 16:53_

(I'm moving this into Stable as resolving the cycle during function inference is more important to resolve.)

---
