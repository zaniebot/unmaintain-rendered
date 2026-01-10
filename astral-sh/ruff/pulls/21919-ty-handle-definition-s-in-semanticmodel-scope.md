```yaml
number: 21919
title: "[ty] Handle `Definition`s in `SemanticModel::scope`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/model-scope-definitions
created_at: 2025-12-11T15:15:53Z
updated_at: 2025-12-11T18:04:58Z
url: https://github.com/astral-sh/ruff/pull/21919
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Handle `Definition`s in `SemanticModel::scope`

---

_Pull request opened by @MichaReiser on 2025-12-11 15:15_

## Summary

I called `definitions_for_name`, where the node was a function definition, and I was really 
surprised that it returned no results, ever... 

The issue was that `SemanticModel::scope` currently only handles expressions and identifiers.

This PR adds the necessary handling to lookup the scope in which the function, class, ... is defined.

## Test Plan

I don't think this is used anywhere today. But seems a footgun not to have from an API standpoint.


---

_Review requested from @carljm by @MichaReiser on 2025-12-11 15:15_

---

_Label `internal` added by @MichaReiser on 2025-12-11 15:15_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-11 15:15_

---

_Label `ty` added by @MichaReiser on 2025-12-11 15:15_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-11 15:15_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-11 15:15_

---

_Label `internal` added by @MichaReiser on 2025-12-11 15:15_

---

_Label `ty` added by @MichaReiser on 2025-12-11 15:15_

---

_Renamed from "[ty] Handle definitions in `SemanticModel::scope`" to "[ty] Handle `Definition`s in `SemanticModel::scope`" by @MichaReiser on 2025-12-11 15:17_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @AlexWaygood on 2025-12-11 15:20_

There are other nodes that can create definitions: `for` loops, `match` patterns, `with` statements, etc. But idk if you were trying to be exhaustive :-)

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5122 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @MichaReiser on 2025-12-11 15:24_

> There are other nodes that can create definitions: for loops, match patterns, with statements, etc. But idk if you were trying to be exhaustive :-)

I added the nodes that support `inferred_type`. I don't think we need special handling for `for` because the target is an `ExprName`. Same for `match` where the target is an `Identifier` (I think). 



---

_Comment by @AlexWaygood on 2025-12-11 15:27_

Makes sense, thanks. It might be worth adding a comment to the `match` statement that explains why some definition nodes have their own branches while many others don't 

---

_@Gankra approved on 2025-12-11 15:41_

IDE's always happy for more of these kinds of lookups to work.

---

_Merged by @MichaReiser on 2025-12-11 18:04_

---

_Closed by @MichaReiser on 2025-12-11 18:04_

---

_Branch deleted on 2025-12-11 18:04_

---
