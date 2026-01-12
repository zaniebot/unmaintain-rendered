```yaml
number: 21793
title: "[ty] Add tests for implicit submodule references"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: gankra/from-ide
created_at: 2025-12-04T14:55:08Z
updated_at: 2025-12-04T15:46:24Z
url: https://github.com/astral-sh/ruff/pull/21793
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Add tests for implicit submodule references

---

_@Gankra_

## Summary

I realized we don't really test `DefinitionKind::ImportFromSubmodule` in the IDE at all, so here's a bunch of them, just recording our current behaviour.

## Test Plan

*stares at the camera*


---

_Review requested from @carljm by @Gankra on 2025-12-04 14:55_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-04 14:55_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-04 14:55_

---

_Review requested from @sharkdp by @Gankra on 2025-12-04 14:55_

---

_Label `internal` added by @Gankra on 2025-12-04 14:55_

---

_Review requested from @dcreager by @Gankra on 2025-12-04 14:55_

---

_Label `ty` added by @Gankra on 2025-12-04 14:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 14:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 14:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 39 diagnostics
+ Found 38 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5513 diagnostics
+ Found 5514 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-12-04 15:30_

Added comments to every test result explaining if I think it's correct or incorrect (all the wrong ones have `TODO(submodule-imports)`)

---

_Comment by @Gankra on 2025-12-04 15:31_

CC @MichaReiser I don't think your import work necessarily needs to fix any of these but this might merge-conflict/snapshot-conflict you.

---

_Merged by @Gankra on 2025-12-04 15:46_

---

_Closed by @Gankra on 2025-12-04 15:46_

---

_Branch deleted on 2025-12-04 15:46_

---
