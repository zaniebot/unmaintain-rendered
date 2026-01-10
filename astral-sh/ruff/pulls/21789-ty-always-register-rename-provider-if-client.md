```yaml
number: 21789
title: "[ty] Always register rename provider if client doesn't support dynamic registration"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/rename-registration
created_at: 2025-12-04T11:04:44Z
updated_at: 2025-12-04T13:40:18Z
url: https://github.com/astral-sh/ruff/pull/21789
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Always register rename provider if client doesn't support dynamic registration

---

_Pull request opened by @MichaReiser on 2025-12-04 11:04_

## Summary

Always register the rename provider if the client doesn't support dynamic registrations so that users don't need to know that renaming support can only be configured using `initialize_options`.

Fixes https://github.com/astral-sh/ty/issues/1745


## Test Plan

I removed the dynamic registration check in `server_capabilities` and tested that symbol renaming isn't
working in VS Code when disabling `rename` but is working when enabled. I also verified
that VS Code uses the rename functionality of any other available LSP (it doesn't block renaming if one LSP returns `None` and an other returns `Some`).


---

_Review requested from @carljm by @MichaReiser on 2025-12-04 11:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-04 11:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-04 11:04_

---

_Label `server` added by @MichaReiser on 2025-12-04 11:04_

---

_Label `ty` added by @MichaReiser on 2025-12-04 11:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 11:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 11:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 38 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@dhruvmanila approved on 2025-12-04 13:36_

Thank you! Good to see the `server_capabilities` getting simpler.

---

_Merged by @MichaReiser on 2025-12-04 13:40_

---

_Closed by @MichaReiser on 2025-12-04 13:40_

---

_Branch deleted on 2025-12-04 13:40_

---
