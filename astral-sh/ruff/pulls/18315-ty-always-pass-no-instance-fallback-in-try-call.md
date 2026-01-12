```yaml
number: 18315
title: "[ty] Always pass `NO_INSTANCE_FALLBACK` in `try_call_dunder_with_policy`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/always-pass-no_instance_fallback
created_at: 2025-05-26T11:06:37Z
updated_at: 2025-05-26T11:20:29Z
url: https://github.com/astral-sh/ruff/pull/18315
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Always pass `NO_INSTANCE_FALLBACK` in `try_call_dunder_with_policy`

---

_@sharkdp_

## Summary

The previous `try_call_dunder_with_policy` API was a bit of a footgun since you needed to pass `NO_INSTANCE_FALLBACK` in *addition* to other policies that you wanted for the member lookup. Implicit calls to dunder methods never access instance members though, so we can do this implicitly in `try_call_dunder_with_policy`.

No functional changes.

---

_Review requested from @carljm by @sharkdp on 2025-05-26 11:06_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-26 11:06_

---

_Review requested from @dcreager by @sharkdp on 2025-05-26 11:06_

---

_Label `ty` added by @sharkdp on 2025-05-26 11:06_

---

_Comment by @github-actions[bot] on 2025-05-26 11:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-05-26 11:15_

---

_Merged by @sharkdp on 2025-05-26 11:20_

---

_Closed by @sharkdp on 2025-05-26 11:20_

---

_Branch deleted on 2025-05-26 11:20_

---
