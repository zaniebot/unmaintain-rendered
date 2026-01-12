```yaml
number: 21453
title: "[ty] Fixup a few details around version-specific dataclass features"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dataclass-params-version
created_at: 2025-11-14T14:09:16Z
updated_at: 2025-11-15T10:21:25Z
url: https://github.com/astral-sh/ruff/pull/21453
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Fixup a few details around version-specific dataclass features

---

_@AlexWaygood_

## Summary

- `slots=True` and `match_args=True` were both added in Python 3.10
- `weakref_slot=True` was added in Python 3.11

I also added a test that shows the types we display when providing autocompletions for instances of `NamedTuple` classes; that came up in https://github.com/astral-sh/ruff/pull/21446

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @AlexWaygood on 2025-11-14 14:09_

---

_Label `ty` added by @AlexWaygood on 2025-11-14 14:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-14 14:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-14 14:09_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-14 14:09_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 14:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 14:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser approved on 2025-11-14 15:01_

---

_Merged by @AlexWaygood on 2025-11-14 15:04_

---

_Closed by @AlexWaygood on 2025-11-14 15:04_

---

_Branch deleted on 2025-11-14 15:04_

---

_Comment by @sharkdp on 2025-11-15 10:21_

Thanks

---
