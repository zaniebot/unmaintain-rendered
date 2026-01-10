```yaml
number: 20557
title: "[ty] Explicitly test assignability/subtyping between unions of nominal types and protocols with method members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/proto-union-tests
created_at: 2025-09-24T16:59:29Z
updated_at: 2025-09-25T09:21:30Z
url: https://github.com/astral-sh/ruff/pull/20557
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Explicitly test assignability/subtyping between unions of nominal types and protocols with method members

---

_Pull request opened by @AlexWaygood on 2025-09-24 16:59_

(Stacked on top of https://github.com/astral-sh/ruff/pull/20555; review that PR first!)

## Summary

@sharkdp very reasonably asked in https://github.com/astral-sh/ruff/pull/20165#discussion_r2348190613 whether the `invoke_descriptor_protocol` call here would produce the right result when testing assignability between a union of nominal types and a protocol with a method member: https://github.com/astral-sh/ruff/blob/83f80effecf682a487abfcd101ea6bbab3471740/crates/ty_python_semantic/src/types/protocol_class.rs#L559-L570

This wasn't something we had explicit tests for, so this PR adds some tests for it!



---

_Review requested from @carljm by @AlexWaygood on 2025-09-24 16:59_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-24 16:59_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-24 16:59_

---

_Label `testing` added by @AlexWaygood on 2025-09-24 16:59_

---

_Label `ty` added by @AlexWaygood on 2025-09-24 16:59_

---

_@sharkdp approved on 2025-09-25 08:17_

Thank you!

---

_Merged by @AlexWaygood on 2025-09-25 09:21_

---

_Closed by @AlexWaygood on 2025-09-25 09:21_

---

_Branch deleted on 2025-09-25 09:21_

---
