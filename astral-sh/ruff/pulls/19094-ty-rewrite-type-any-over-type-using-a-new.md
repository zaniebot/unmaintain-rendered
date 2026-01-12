```yaml
number: 19094
title: "[ty] Rewrite `Type::any_over_type` using a new generalised `TypeVisitor` trait"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/type-visitor
created_at: 2025-07-02T15:05:37Z
updated_at: 2025-07-03T18:19:53Z
url: https://github.com/astral-sh/ruff/pull/19094
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Rewrite `Type::any_over_type` using a new generalised `TypeVisitor` trait

---

_@AlexWaygood_

## Summary

Our `Type::any_over_type()` method has two problems:
1. There are several types where it does not recurse into all nested types!
2. It doesn't guard against recursion at all. This isn't much of a problem now, but becomes more of a problem when we introduce more recursive types, such as in https://github.com/astral-sh/ruff/pull/18659

This PR introduces a new `TypeVisitor` trait that aims to solve both of these issues in a generalized way:
1. Walking nested types is delegated down to `walk_*_type` functions that live close to the structs they walk. For example, walking all nested types in a `NominalInstanceType` is handled by a `walk_nominal_instance_type` function that lives next to the `NominalInstanceType` struct in `instance.rs`. This makes it less likely that we'll forget to update the function if we add a new field to `NominalInstanceType` in the future.
2. The trait makes it easy to write concrete implementations that guard against recursion in an efficient way.

`Type::any_over_type` is reimplemented as a standalone `any_over_type` function that uses a concrete implementation of the new `TypeVisitor` trait as its implementation.

If we like the direction this PR is going in, we might want to rename the existing `TypeVisitor` struct that @carljm added in https://github.com/astral-sh/ruff/pull/19003. We _may_ also want to reimplement `Type::normalize` and `Type::apply_type_mapping` using a similar `TypeTransformer` trait, but I'll defer to @carljm on that point, since he has ongoing work in this area.

## Test Plan

All existing tests pass.


---

_Review requested from @carljm by @AlexWaygood on 2025-07-02 15:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-02 15:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-02 15:05_

---

_Label `internal` added by @AlexWaygood on 2025-07-02 15:07_

---

_Label `ty` added by @AlexWaygood on 2025-07-02 15:07_

---

_Comment by @github-actions[bot] on 2025-07-02 15:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~54MB
+     memo fields = ~49MB

operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~80MB
+     memo fields = ~88MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

discord.py (https://github.com/Rapptz/discord.py)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~251MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

mkosi (https://github.com/systemd/mkosi)
-     memo fields = ~106MB
+     memo fields = ~97MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~368MB
+ TOTAL MEMORY USAGE: ~334MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~66MB
+     memo fields = ~72MB

paasta (https://github.com/yelp/paasta)
- TOTAL MEMORY USAGE: ~189MB
+ TOTAL MEMORY USAGE: ~207MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~276MB

```
</details>


---

_Review comment by @carljm on `crates/ty_python_semantic/src/lib.rs`:46 on 2025-07-02 17:49_

I wonder if some of our existing uses of `FxOrderSet` should actually be `FxIndexSet`, if we don't need the stronger equality and order-maintenance guarantees provided by the `ordermap` wrapper crate? Not something for this PR, though.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7524 on 2025-07-02 18:00_

These are the sorts of cases that made me somewhat hesitant to implement a generic walk-types facility (or at least, unclear how it should be implemented). Is the RHS of a PEP695 type alias "part of" its type? It's certainly relevant to the meaning of the type. Is the answer the same for every possible type walk? I would probably expect `any_over_type` to descend into the RHS, but I'm not sure what a type transformer could be expected to do here, unless we implemented a "Synthesized" variant of PEP 695 type aliases. (Maybe we will need that in order to support recursive type aliases?)

It seems somewhat parallel to the case of class-defined (non-synthesized) protocols, where the semantics of the type includes details that require further queries and aren't stored directly in the type itself.

I think this is fine for this PR, just musing.

---

_@carljm reviewed on 2025-07-02 18:52_

The code here looks nice. My main hesitation here is that I'm not totally clear that `any_over_type` is even a clearly defined operation for all types (see inline comment), and it's only currently used in deciding whether to issue a redundant-cast diagnostic. This seems like a lot of machinery to introduce for that small edge case, in the absence of a clear understanding (which I at least don't currently have) of the semantics of the generalized operation, and the future use cases for it.

I'm tempted to say that we should instead try to remove the need for `any_over_type` (as we already did for `is_fully_static`), and aim to avoid the need for these generalized recursive type walk tests entirely. To me, the ecosystem report on https://github.com/astral-sh/ruff/pull/19099 suggests that this is feasible. Removing the use of `any_over_type` entirely does not fail any existing tests or introduce many new diagnostics, and it seems like a fairly limited effort to address some Todos would remove most of the diagnostics it does introduce. (Plus, I don't think the current existence of todos and the desire to silence false positives arising from them should drive significant design decisions.)

I do think this PR is useful regardless, because I think a lot of it could be reused for a similar `TypeTransformer` trait, and that's something I do think we will need.

---

_Comment by @carljm on 2025-07-02 19:09_

Thank you for putting this together! I think my current feeling is we should go with https://github.com/astral-sh/ruff/pull/19099 instead.

---

_@AlexWaygood reviewed on 2025-07-03 11:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/lib.rs`:46 on 2025-07-03 11:40_

I don't think an `FxIndexSet` is hashable, so I think we do need to use `FxOrderSet` for the fields on `IntersectionType`, for example. But yeah, it does look like a bunch of places are using an `OrderSet` right now when they probably only really need an `IndexSet`.

---

_@carljm reviewed on 2025-07-03 16:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7524 on 2025-07-03 16:36_

Follow-up from in-person discussion: this should be fixed to walk the RHS

---

_@carljm approved on 2025-07-03 16:40_

After discussing in person, I'm convinced that we will likely have other future uses for this. I don't love that we do this potentially-expensive walk on two different types every time we see a `cast`, but it doesn't seem in practice like that's causing noticeable perf issues on any ecosystem project; we can deal with it later if it comes up.

So this looks good to me, modulo the PEP 695 type aliases fix.

---

_Comment by @carljm on 2025-07-03 16:41_

Oh, and I do think we should rename my existing `TypeVisitor` to avoid confusion, either in this PR or as a follow-up. It could already be called `TypeTransformer` maybe?

---

_Merged by @AlexWaygood on 2025-07-03 18:19_

---

_Closed by @AlexWaygood on 2025-07-03 18:19_

---

_Branch deleted on 2025-07-03 18:19_

---
