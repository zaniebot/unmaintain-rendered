```yaml
number: 21341
title: "[ty] Fix `--exclude` and `src.exclude` merging"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - cli
  - ty
assignees: []
merged: true
base: main
head: micha/ranged-value-combine
created_at: 2025-11-08T15:55:19Z
updated_at: 2025-11-10T11:52:46Z
url: https://github.com/astral-sh/ruff/pull/21341
synced_at: 2026-01-12T15:57:21Z
```

# [ty] Fix `--exclude` and `src.exclude` merging

---

_@MichaReiser_

## Summary

The `RangedValue` `Combine` implementation always returned `self` 
instead of using `T::combine` to merge the value. 

This results in incorrect merging for lists (and maps) where the values should be
merged and not replaced because we use the `!pattern` syntax to override the inherited `pattern`.


## Test Plan

Added test


---

_Label `ci` added by @MichaReiser on 2025-11-08 15:55_

---

_Review requested from @carljm by @MichaReiser on 2025-11-08 15:55_

---

_Label `ty` added by @MichaReiser on 2025-11-08 15:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-08 15:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-08 15:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-08 15:55_

---

_Comment by @astral-sh-bot[bot] on 2025-11-08 15:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-08 15:58_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Renamed from "[ty] Fix merging of `--exclude` with `src.exclude`" to "[ty] Fix `--exclude` and `src.exclude` merging" by @MichaReiser on 2025-11-08 16:11_

---

_Label `ci` removed by @MichaReiser on 2025-11-08 16:11_

---

_Label `configuration` added by @MichaReiser on 2025-11-08 16:11_

---

_Label `cli` added by @MichaReiser on 2025-11-08 16:11_

---

_@sharkdp approved on 2025-11-09 18:44_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-09 18:57_

---

_Merged by @MichaReiser on 2025-11-10 11:52_

---

_Closed by @MichaReiser on 2025-11-10 11:52_

---

_Branch deleted on 2025-11-10 11:52_

---
