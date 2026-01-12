```yaml
number: 17047
title: "[red-knot] Allow `CallableTypeFromFunction` to display the signatures of callable types that are not function literals"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/callabletypefrommethod
created_at: 2025-03-28T19:29:47Z
updated_at: 2025-03-30T18:04:36Z
url: https://github.com/astral-sh/ruff/pull/17047
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Allow `CallableTypeFromFunction` to display the signatures of callable types that are not function literals

---

_@AlexWaygood_

I found this helpful for understanding some of the stuff that was going on in https://github.com/astral-sh/ruff/pull/17005. It means the name of the special form is perhaps no longer ideal; I could possibly rename it if we agree that this is a useful feature.

---

_Review requested from @carljm by @AlexWaygood on 2025-03-28 19:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-28 19:29_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-28 19:29_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-28 19:29_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-03-28 19:31_

---

_Comment by @github-actions[bot] on 2025-03-28 19:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-03-28 19:42_

This looks good to me! I would probably rename to `CallableTypeOf`?

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-28 20:19_

---

_Comment by @AlexWaygood on 2025-03-28 20:20_

Renamed it to `CallableTypeOf`!

---

_Merged by @AlexWaygood on 2025-03-28 20:23_

---

_Closed by @AlexWaygood on 2025-03-28 20:23_

---

_Branch deleted on 2025-03-28 20:23_

---

_@dhruvmanila reviewed on 2025-03-28 20:32_

Looks good 👍 

---

_@MatthewMckee4 reviewed on 2025-03-29 13:02_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:457 on 2025-03-29 13:02_

Should c5 be stored as `(x: int) -> str`, or stored as `(self, x: int) -> str` and treated like `(x: int) -> str`?

@AlexWaygood @carljm 

---

_@carljm reviewed on 2025-03-30 18:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:457 on 2025-03-30 18:04_

Not entirely sure! I think this requires deeper exploration. Today we do the latter. There are a couple interesting questions that it connects to:

1) How do we handle equivalent callable types? If we aim for "equivalent callable types are always the same Salsa ID" then it should actually be stored as `(x: int) -> str`. But I don't think it's totally clear yet if that will be feasible.

2) When do we issue an error on calling a bound method with an annotation on the `self` argument that doesn't match the instance we accessed it from? Does it happen when accessing the bound method, or when calling it? The latter seems better (matches runtime better), but requires storing the bound self type and the full signature.

It seems likely that we need to support both representations and understand their equivalence. (But maybe our "general callable type" uses only the simpler representation?)

---
