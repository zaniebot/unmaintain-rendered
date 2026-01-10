```yaml
number: 19003
title: "[ty] Normalize recursive types using Any"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cyclicp
created_at: 2025-06-27T22:15:48Z
updated_at: 2025-07-02T17:18:15Z
url: https://github.com/astral-sh/ruff/pull/19003
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Normalize recursive types using Any

---

_Pull request opened by @carljm on 2025-06-27 22:15_

## Summary

This just replaces one temporary solution to recursive protocols (the `SelfReference` mechanism) with another one (track seen types when recursively descending in `normalize` and replace recursive references with `Any`). But this temporary solution can handle mutually-recursive types, not just self-referential ones, and it's sufficient for the primer ecosystem and some other projects we are testing on to no longer stack overflow.

The follow-up here will be to properly handle these self-references instead of replacing them with `Any`.

We will also eventually need cycle detection on more recursive-descent type transformations and tests.

## Test Plan

Existing tests (including recursive-protocol tests) and primer.

Added mdtest for mutually-recursive protocols that stack-overflowed before this PR.


---

_Label `ty` added by @carljm on 2025-06-27 22:15_

---

_Comment by @github-actions[bot] on 2025-06-27 22:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-argument-type] strawberry/schema_codegen/__init__.py:199:34: Argument to function `_get_directives` is incorrect: Expected `HasDirectives`, found `FieldDefinitionNode | InputValueDefinitionNode`
- error[invalid-argument-type] strawberry/schema_codegen/__init__.py:387:34: Argument to function `_get_directives` is incorrect: Expected `HasDirectives`, found `ObjectTypeDefinitionNode | ObjectTypeExtensionNode | InterfaceTypeDefinitionNode | InputObjectTypeDefinitionNode`
- Found 366 diagnostics
+ Found 364 diagnostics

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB

```
</details>


---

_Marked ready for review by @carljm on 2025-06-27 22:35_

---

_Review requested from @AlexWaygood by @carljm on 2025-06-27 22:35_

---

_Review requested from @sharkdp by @carljm on 2025-06-27 22:35_

---

_Review requested from @dcreager by @carljm on 2025-06-27 22:35_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1082 on 2025-06-28 06:33_

Can we avoid inserting for types that are known not to be recursive (eg literals) or can all types be recursive?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1084 on 2025-06-28 06:36_

Should this be a todo type instead of any

---

_@MichaReiser approved on 2025-06-28 06:36_

Nice

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7263 on 2025-06-28 13:15_

what's the reason for dropping the doc-comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1141 on 2025-06-28 13:17_

we do a similar thing in the `inheritance_cycle` query in `class.rs` but there we use an `IndexSet` so that we can just pop the most recently added item off the end of the set:

https://github.com/astral-sh/ruff/blob/90cb0d3a7b0d1b9528854dad64dd17330bfd5f36/crates/ty_python_semantic/src/types/class.rs#L2148

I don't know if that's actually more efficient than using `FxHashSet::remove`, though

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5764 on 2025-06-28 13:21_

do we also need to create a `TypeAliasType::normalized_impl` method that passes down `seen_types`?

---

_@AlexWaygood approved on 2025-06-28 13:26_

This looks great!

---

_@AlexWaygood reviewed on 2025-06-28 14:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1084 on 2025-06-28 14:10_

I think the issue here might be that `Todo` types themselves are normalized to `Any`, because they are all equivalent to `Any` (and the idea of normalization is that all equivalent types share the same Salsa ID after being normalized).

Normalized types should pretty rarely show up in user-facing messages, so there's probably no practical difference between `Todo` and `Any` here anyway.

---

_@AlexWaygood reviewed on 2025-06-28 14:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1084 on 2025-06-28 14:10_

If that is the reason, though, it might be good to add a comment about it!

---

_Comment by @MichaReiser on 2025-06-28 14:21_

Does this unblock the work on protocols?

---

_@carljm reviewed on 2025-06-28 15:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5764 on 2025-06-28 15:51_

Yes, good catch

---

_@carljm reviewed on 2025-06-28 15:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1082 on 2025-06-28 15:52_

We can. It will require a more complex structure for this function, and I think it will require two matches on `self` instead of one. That should still be worth it, if we can avoid extra hashset inserts and removes.

---

_@MichaReiser reviewed on 2025-06-28 16:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1082 on 2025-06-28 16:23_

You could change `SeenType` to have a `SeenType::visit(ty, |ty| ....)` method. Not sure if that's more annoying though.

---

_@carljm reviewed on 2025-06-30 17:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1141 on 2025-06-30 17:38_

The follow up PR (WIP) switches this to an `FxOrderSet` anyway, so we can calculate the right de Bruijn index. I'll just change it in this PR for less churn.

---

_@sharkdp reviewed on 2025-06-30 18:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/primer/bad.txt`:22 on 2025-06-30 18:23_

Thanks, forgot that since it had a different description than the other one

---

_@carljm reviewed on 2025-06-30 18:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1082 on 2025-06-30 18:51_

I made this change (and also renamed `SeenTypes` to `TypeVisitor`). I think it works out ok, not too annoying. Thanks for the suggestion.

---

_Comment by @carljm on 2025-06-30 19:07_

Will go ahead and merge this, but post-land review of the TypeVisitor change is welcome.

(The callback accepted by `TypeVisitor::visit` has to itself take the visitor as argument, because otherwise it would have to be closed-over at the callsite, and this would cause conflicting mutable borrows of the visitor, one for the `visitor.visit` call itself and the other for the closure.)

---

_Merged by @carljm on 2025-06-30 19:07_

---

_Closed by @carljm on 2025-06-30 19:07_

---

_Branch deleted on 2025-06-30 19:07_

---

_Comment by @sharkdp on 2025-07-01 12:28_

> post-land review of the TypeVisitor change is welcome

Looks good, thank you!

---

_Comment by @AlexWaygood on 2025-07-01 12:43_

> The callback accepted by `TypeVisitor::visit` has to itself take the visitor as argument, because otherwise it would have to be closed-over at the callsite, and this would cause conflicting mutable borrows of the visitor, one for the `visitor.visit` call itself and the other for the closure.

You might be able to get around that by having the `seen` field on the visitor struct be `RefCell<FxOrderSet<Type<'db>>>` rather than `FxOrderSet<Type<'db>>`. Then the `visit` method would only need to take `&self` rather than `&mut self`.

Not a big deal either way, though!

---

_Comment by @carljm on 2025-07-02 17:18_

> You might be able to get around that by having the `seen` field on the visitor struct be `RefCell<FxOrderSet<Type<'db>>>` rather than `FxOrderSet<Type<'db>>`. Then the `visit` method would only need to take `&self` rather than `&mut self`.

Yes, I considered interior mutability, but I don't think that's a good tradeoff. It sacrifices some performance (by adding the runtime reference tracking overhead of `RefCell`) for a fairly minor ergonomic win.

---
