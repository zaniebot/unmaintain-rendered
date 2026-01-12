```yaml
number: 21845
title: "[ty] Don't create a related diagnostic for the primary annotation of sub-diagnostics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/filter-primary-annotations
created_at: 2025-12-08T13:48:50Z
updated_at: 2025-12-08T14:22:13Z
url: https://github.com/astral-sh/ruff/pull/21845
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Don't create a related diagnostic for the primary annotation of sub-diagnostics

---

_@MichaReiser_

## Summary

Addresses the 'too many "Python 3.7 was assumed" diagnostics issue raised in [this comment](https://github.com/astral-sh/ty/issues/1582#issuecomment-3626555051)

We already skip the primary annotation when creating related diagnostics for the main diagnostic, but we failed to do so for sub-diagnostics, which results in duplicated related information diagnostics.

We need to skip the primary annotation because we already use that range when generating related information for the subdiagnostic.


## Test Plan

<img width="970" height="305" alt="Screenshot 2025-12-08 at 14 48 57" src="https://github.com/user-attachments/assets/af23f771-5176-4152-a357-0a9b25869609" />



---

_Review requested from @carljm by @MichaReiser on 2025-12-08 13:48_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-08 13:48_

---

_Label `server` added by @MichaReiser on 2025-12-08 13:48_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-08 13:48_

---

_Label `ty` added by @MichaReiser on 2025-12-08 13:48_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-08 13:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 13:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 13:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 38 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/api/rest.py:1047:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2070 diagnostics
+ Found 2072 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 14:05_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@AlexWaygood approved on 2025-12-08 14:11_

Thank you!!

---

_Merged by @MichaReiser on 2025-12-08 14:22_

---

_Closed by @MichaReiser on 2025-12-08 14:22_

---

_Branch deleted on 2025-12-08 14:22_

---
