```yaml
number: 21763
title: "Fix syntax error false positives for `await` outside functions"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/f704
created_at: 2025-12-02T19:57:33Z
updated_at: 2025-12-02T21:02:03Z
url: https://github.com/astral-sh/ruff/pull/21763
synced_at: 2026-01-10T16:48:02Z
```

# Fix syntax error false positives for `await` outside functions

---

_Pull request opened by @ntBre on 2025-12-02 19:57_

## Summary

Fixes #21750 and a related bug in `PLE1142`. We were not properly considering generators to be valid `await` contexts, which caused the `F704` issue. One of the tests I added for this also uncovered an issue in `PLE1142` for comprehensions nested within async generators because we were only checking the current scope rather than traversing the nested context.

## Test Plan

Both of these rules are implemented as semantic syntax errors, so I added tests (and fixes) in both Ruff and ty.

---

_Label `bug` added by @ntBre on 2025-12-02 19:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 20:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 20:02_


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
- Found 45 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 20:15_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @ntBre on 2025-12-02 20:17_

---

_Review requested from @carljm by @ntBre on 2025-12-02 20:17_

---

_Review requested from @AlexWaygood by @ntBre on 2025-12-02 20:17_

---

_Review requested from @sharkdp by @ntBre on 2025-12-02 20:17_

---

_Review requested from @dcreager by @ntBre on 2025-12-02 20:17_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-02 20:17_

---

_Review requested from @dhruvmanila by @ntBre on 2025-12-02 20:17_

---

_Review request for @dcreager removed by @ntBre on 2025-12-02 20:17_

---

_Review request for @carljm removed by @ntBre on 2025-12-02 20:17_

---

_Review request for @sharkdp removed by @ntBre on 2025-12-02 20:17_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-12-02 20:17_

---

_Review request for @dhruvmanila removed by @ntBre on 2025-12-02 20:17_

---

_Review requested from @amyreese by @ntBre on 2025-12-02 20:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2903 on 2025-12-02 20:38_

The `rev` doesn't seem necessary, given that we only care about whether there's any

---

_@MichaReiser approved on 2025-12-02 20:38_

---

_Merged by @ntBre on 2025-12-02 21:02_

---

_Closed by @ntBre on 2025-12-02 21:02_

---

_Branch deleted on 2025-12-02 21:02_

---
