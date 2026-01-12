```yaml
number: 21419
title: "[ty] Rename matching overload index field"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/rename-matching-overload-field
created_at: 2025-11-13T06:07:46Z
updated_at: 2025-11-13T08:37:00Z
url: https://github.com/astral-sh/ruff/pull/21419
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Rename matching overload index field

---

_@dhruvmanila_

## Summary

This PR renames the `CallableBinding::matching_overload_index` field to `CallableBinding::matching_overload_after_parameter_matching` to clarify the main use case of this field which is to surface type checking errors on the matching overloads directly instead of using the `no-matching-overload` diagnostic. This can only happen after parameter matching as following steps could filter out this overload which should then result in `no-matching-overload` diagnostic.

Callers should use the `matching_overload_index` _method_ to get the matching overloads.


---

_Label `internal` added by @dhruvmanila on 2025-11-13 06:07_

---

_Review requested from @carljm by @dhruvmanila on 2025-11-13 06:07_

---

_Label `ty` added by @dhruvmanila on 2025-11-13 06:07_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-11-13 06:07_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-11-13 06:07_

---

_Review requested from @dcreager by @dhruvmanila on 2025-11-13 06:07_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 06:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 06:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@sharkdp approved on 2025-11-13 07:47_

Thank you. Given that it is potentially wrong to access this, maybe we should even choose a name that makes it more apparent that this is not the "final" matching overload? Instead of "after parameter matching", maybe it should be called "before (argument and parameter) type checking" (e.g. `matching_overload_before_type_checks`)?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-13 07:57_

---

_Comment by @dhruvmanila on 2025-11-13 08:32_

Renamed it to `matching_overload_before_type_checking`

---

_Merged by @dhruvmanila on 2025-11-13 08:36_

---

_Closed by @dhruvmanila on 2025-11-13 08:36_

---

_Branch deleted on 2025-11-13 08:37_

---
