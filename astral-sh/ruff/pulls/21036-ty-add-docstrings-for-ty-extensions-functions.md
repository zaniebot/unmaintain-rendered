```yaml
number: 21036
title: "[ty] Add docstrings for `ty_extensions` functions"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ty-extensions-docstrings
created_at: 2025-10-22T22:37:56Z
updated_at: 2025-11-12T23:13:19Z
url: https://github.com/astral-sh/ruff/pull/21036
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Add docstrings for `ty_extensions` functions

---

_Pull request opened by @ericmarkmartin on 2025-10-22 22:37_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add docstrings to some ty_extensions functions that test predicates on types.

Additionally update some keyword argument names in these functions to clarify which direction the relevant relations are being tested.

## Test Plan

Manually tested with a local ty language server that the new docstrings show up.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-22 22:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `internal` added by @AlexWaygood on 2025-10-22 22:41_

---

_Label `ty` added by @AlexWaygood on 2025-10-22 22:41_

---

_Comment by @github-actions[bot] on 2025-10-22 22:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @ericmarkmartin on 2025-10-22 22:56_

---

_Review requested from @carljm by @ericmarkmartin on 2025-10-22 22:56_

---

_Review requested from @MichaReiser by @ericmarkmartin on 2025-10-22 22:56_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-10-22 22:56_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-10-22 22:56_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-10-22 22:56_

---

_Review comment by @sharkdp on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:94 on 2025-10-23 07:29_

```suggestion
    """Returns `True` if `ty` is non-empty and all inhabitants compare equal to each other."""
```

---

_Review comment by @sharkdp on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:90 on 2025-10-23 07:31_

I think this conflates "single inhabitant" with "single value" (which is what `is_single_valued` checks), so maybe just:
```suggestion
    """Returns `True` if `ty` is a singleton type with exactly one inhabitant."""
```

---

_@sharkdp approved on 2025-10-23 07:31_

Thank you very much!

---

_@sharkdp reviewed on 2025-10-23 07:32_

---

_Review comment by @sharkdp on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:89 on 2025-10-23 07:32_

```suggestion
```

---

_Renamed from "[ty] Add docstrings to some ty_extensions functions" to "[ty] Add docstrings for `ty_extensions` functions" by @sharkdp on 2025-10-23 07:40_

---

_Merged by @MichaReiser on 2025-10-23 07:50_

---

_Closed by @MichaReiser on 2025-10-23 07:50_

---

_Branch deleted on 2025-11-12 23:13_

---
