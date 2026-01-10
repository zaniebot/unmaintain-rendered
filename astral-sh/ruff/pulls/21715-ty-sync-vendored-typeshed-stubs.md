```yaml
number: 21715
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-12-01T00:59:10Z
updated_at: 2025-12-03T15:50:09Z
url: https://github.com/astral-sh/ruff/pull/21715
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-12-01 00:59_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-12-01 00:59_

---

_Label `ty` added by @github-actions[bot] on 2025-12-01 00:59_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-12-01 00:59_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-12-01 00:59_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-12-01 00:59_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-12-01 00:59_

---

_Closed by @AlexWaygood on 2025-12-01 00:59_

---

_Reopened by @AlexWaygood on 2025-12-01 01:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 01:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 01:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5511 diagnostics
+ Found 5512 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-01 07:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 07:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-base` | 1 | 2 | 0 |
| **Total** | **1** | **2** | **0** |

**[Full report with detailed diff](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/diff)** ([timing results](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/timing))




---

_Comment by @dcreager on 2025-12-01 23:40_

https://github.com/astral-sh/ruff/pull/21744 should fix the macOS CI failure

---

_Closed by @sharkdp on 2025-12-02 08:18_

---

_Reopened by @sharkdp on 2025-12-02 08:18_

---

_Closed by @dcreager on 2025-12-03 15:20_

---

_Reopened by @dcreager on 2025-12-03 15:20_

---

_Merged by @AlexWaygood on 2025-12-03 15:49_

---

_Closed by @AlexWaygood on 2025-12-03 15:49_

---

_Branch deleted on 2025-12-03 15:49_

---

_Comment by @AlexWaygood on 2025-12-03 15:50_

Thank you @dcreager!!

---
