```yaml
number: 19756
title: "[ty] Keep track of type qualifiers in stub declarations without right-hand side"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-937
created_at: 2025-08-05T08:42:54Z
updated_at: 2025-08-05T10:07:07Z
url: https://github.com/astral-sh/ruff/pull/19756
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Keep track of type qualifiers in stub declarations without right-hand side

---

_Pull request opened by @sharkdp on 2025-08-05 08:42_

## Summary

closes https://github.com/astral-sh/ty/issues/937

## Test Plan

Regression test

---

_Label `bug` added by @sharkdp on 2025-08-05 08:42_

---

_Label `ty` added by @sharkdp on 2025-08-05 08:42_

---

_Comment by @github-actions[bot] on 2025-08-05 08:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree//conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 08:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-08-05 08:48_

---

_Review requested from @carljm by @sharkdp on 2025-08-05 08:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-05 08:48_

---

_Review requested from @dcreager by @sharkdp on 2025-08-05 08:48_

---

_Comment by @sharkdp on 2025-08-05 10:07_

This is really just an oversight, and the changes seem uncontroversial, so I don't think it's valuable if someone reviews this.

---

_Merged by @sharkdp on 2025-08-05 10:07_

---

_Closed by @sharkdp on 2025-08-05 10:07_

---

_Branch deleted on 2025-08-05 10:07_

---
