```yaml
number: 19802
title: "[ty] fix incorrect member narrowing"
type: pull_request
state: merged
author: mtshiba
labels: []
assignees: []
merged: true
base: main
head: fix-incorrect-member-narrowing
created_at: 2025-08-07T11:35:55Z
updated_at: 2025-08-13T04:02:39Z
url: https://github.com/astral-sh/ruff/pull/19802
synced_at: 2026-01-10T17:52:17Z
```

# [ty] fix incorrect member narrowing

---

_Pull request opened by @mtshiba on 2025-08-07 11:35_

## Summary

Reported in: https://github.com/astral-sh/ruff/pull/19795#issuecomment-3161981945

If a root expression is reassigned, narrowing on the member should be invalidated, but there was an oversight in the current implementation.

This PR fixes that, and also removes some unnecessary handling.

## Test Plan

New tests cases in `narrow/conditionals/nested.md`.


---

_Review requested from @carljm by @mtshiba on 2025-08-07 11:35_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-08-07 11:35_

---

_Review requested from @sharkdp by @mtshiba on 2025-08-07 11:35_

---

_Review requested from @dcreager by @mtshiba on 2025-08-07 11:35_

---

_Comment by @github-actions[bot] on 2025-08-07 11:37_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-07 11:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/webscanner_helper/test_proxyauth_selenium.py:49:9: warning[possibly-unbound-attribute] Attribute `close` on type `Unknown | None` is possibly unbound
- Found 1821 diagnostics
+ Found 1822 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/optimize/tests/test__differential_evolution.py:289:49: error[unresolved-attribute] Type `def func(x) -> Unknown` has no attribute `x`
- scipy/optimize/tests/test__differential_evolution.py:290:51: error[unresolved-attribute] Type `def func(x) -> Unknown` has no attribute `val`
- Found 6796 diagnostics
+ Found 6794 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~80MB
+     memo metadata = ~76MB
-     memo fields = ~445MB
+     memo fields = ~424MB

```
</details>


---

_@mtshiba reviewed on 2025-08-07 11:40_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:300 on 2025-08-07 11:40_

The reason this is `Unknown` is because lazy snapshots only record bindings, not declarations.
That is, `l` is inferred to be `list[Unknown]` based on the binding.

Whether to include declaration information in the lazy snapshot will be an item for future consideration.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:300 on 2025-08-07 23:00_

I think we should infer the type `list[str | None]` for `l` in the binding above, this is just incomplete work in our generics implementation.

---

_@carljm approved on 2025-08-07 23:03_

---

_Merged by @carljm on 2025-08-07 23:04_

---

_Closed by @carljm on 2025-08-07 23:04_

---

_Branch deleted on 2025-08-13 04:02_

---
