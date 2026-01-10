```yaml
number: 21909
title: "[ty] avoid unions of generic aliases of the same class in fixpoint"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/nounion
created_at: 2025-12-10T23:43:37Z
updated_at: 2025-12-11T17:53:46Z
url: https://github.com/astral-sh/ruff/pull/21909
synced_at: 2026-01-10T16:42:11Z
```

# [ty] avoid unions of generic aliases of the same class in fixpoint

---

_Pull request opened by @carljm on 2025-12-10 23:43_

Partially addresses https://github.com/astral-sh/ty/issues/1732
Fixes https://github.com/astral-sh/ty/issues/1800

## Summary

At each fixpoint iteration, we union the "previous" and "current" iteration types, to ensure that the type can only widen at each iteration. This prevents oscillation and ensures convergence.

But some unions triggered by this behavior (in particular, unions of differently-specialized generic-aliases of the same class) never simplify, and cause spurious errors. Since we haven't seen examples of oscillating types involving class-literal or generic-alias types, just don't union those.

There may be more thorough/principled ways to avoid undesirable unions in fixpoint iteration, but this narrow change seems like it results in strict improvement.

## Test Plan

Removes two false positive `unsupported-class-base` in mdtests, and several in the ecosystem, without causing other regression.


---

_Label `ty` added by @carljm on 2025-12-10 23:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 23:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 23:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/channels/handlers/ws_handler.py:69:5: warning[unsupported-base] Unsupported class base with type `<class 'AsyncBaseHTTPView[GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, Context@GraphQLWSConsumer, RootValue@GraphQLWSConsumer]'> | <class 'AsyncBaseHTTPView[GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], Context@GraphQLWSConsumer, RootValue@GraphQLWSConsumer]'>`
- Found 402 diagnostics
+ Found 401 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5122 diagnostics
+ Found 5123 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @carljm on 2025-12-11 00:09_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-11 00:09_

---

_Review requested from @sharkdp by @carljm on 2025-12-11 00:09_

---

_Review requested from @dcreager by @carljm on 2025-12-11 00:09_

---

_Comment by @carljm on 2025-12-11 00:12_

@mtshiba I can't request your review, but would be happy for you to take a look at this.

---

_@mtshiba reviewed on 2025-12-11 02:19_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:920 on 2025-12-11 02:19_

We should probably make sure the class/generic alias identities match.


---

_Comment by @AlexWaygood on 2025-12-11 12:18_

Have we actually seen any issues reported due to unions of _class-literal_ types from our fixpoint machinery? Or has it just been generic-alias types?

I'm not sure how we ever would get a spurious union of _class-literal_ types from our fixpoint machinery. Though if we did, I agree it could cause similar issues to the ones we've seen in the ecosystem (and in issue reports) regarding generic-alias types 

---

_@MichaReiser approved on 2025-12-11 12:30_

Seems worth a try and, at the very least, would help us learn where our assumption is off

---

_@AlexWaygood reviewed on 2025-12-11 12:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:920 on 2025-12-11 12:32_

I think if they are both generic aliases, we should check that the class-literal origin of the alias is the same. If they're both class-literal types and they're the same, then the union will simplify anyway, so there's no issue.

---

_@mtshiba reviewed on 2025-12-11 15:23_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:920 on 2025-12-11 15:23_

The result's class literal type may [flip between cycles](https://github.com/astral-sh/ruff/blob/c9155d5e7233ee3ccc42c9846d5c7e525c06f40f/crates/ty_python_semantic/resources/mdtest/attributes.md?plain=1#L2387-L2403): two different types must be unioned, the current type alone does not keep the query monotonic.

---

_@carljm reviewed on 2025-12-11 16:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:920 on 2025-12-11 16:10_

In that example I don't think any class-literal type itself ever oscillates (or even changes). Class-literal types are quite simple; they don't include attribute types, which are looked up separately. Even bases are looked up separately from the identity of the class-literal itself. I think Alex is right that we've never seen any occasion for a class-literal type to oscillate (or even change) in a cycle, only generic alias types in class bases.

---

_@AlexWaygood approved on 2025-12-11 17:26_

---

_Renamed from "[ty] avoid unions of class literals / generic aliases in fixpoint" to "[ty] avoid unions of generic aliases of the same class in fixpoint" by @carljm on 2025-12-11 17:28_

---

_Merged by @carljm on 2025-12-11 17:53_

---

_Closed by @carljm on 2025-12-11 17:53_

---

_Branch deleted on 2025-12-11 17:53_

---
