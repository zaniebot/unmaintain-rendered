```yaml
number: 21955
title: "[ty] update implicit root docs"
type: pull_request
state: merged
author: Gankra
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: gankra/rootdoc
created_at: 2025-12-12T18:21:48Z
updated_at: 2025-12-12T21:30:25Z
url: https://github.com/astral-sh/ruff/pull/21955
synced_at: 2026-01-12T15:57:37Z
```

# [ty] update implicit root docs

---

_@Gankra_

## Summary

./tests is now no longer an implicit root, per https://github.com/astral-sh/ruff/pull/21817



---

_Label `documentation` added by @Gankra on 2025-12-12 18:21_

---

_Label `ty` added by @Gankra on 2025-12-12 18:21_

---

_Marked ready for review by @Gankra on 2025-12-12 18:21_

---

_Review requested from @carljm by @Gankra on 2025-12-12 18:21_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-12 18:21_

---

_Review requested from @sharkdp by @Gankra on 2025-12-12 18:21_

---

_Review requested from @dcreager by @Gankra on 2025-12-12 18:21_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 18:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 18:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-12-12 21:03_

---

_Merged by @Gankra on 2025-12-12 21:30_

---

_Closed by @Gankra on 2025-12-12 21:30_

---

_Branch deleted on 2025-12-12 21:30_

---
