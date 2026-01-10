```yaml
number: 16861
title: "[red-knot] detect non-deferred self-references in annotations"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
base: main
head: self-reference-annotation
created_at: 2025-03-20T04:40:57Z
updated_at: 2025-03-24T14:29:28Z
url: https://github.com/astral-sh/ruff/pull/16861
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] detect non-deferred self-references in annotations

---

_Pull request opened by @mtshiba on 2025-03-20 04:40_

## Summary

This PR closes #16341.

## Test Plan

New test cases are added in `annotations/deferred.md`.


---

_Review requested from @carljm by @mtshiba on 2025-03-20 04:40_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-20 04:40_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-20 04:40_

---

_Review requested from @dcreager by @mtshiba on 2025-03-20 04:40_

---

_Comment by @github-actions[bot] on 2025-03-20 04:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-20 07:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5906 on 2025-03-20 10:58_

Hmm, I don't think special-casing the immediate enclosing class here is the correct fix. With this PR branch, red-knot emits a diagnostic on the `x` annotation in the `Foo.Bar.f` method in the following example, but we do not emit an error on the `x` annotation in the `Foo.Bar.g` method. Both annotations raise `NameError` at runtime:

```py
class Foo:
    class Bar:
        def f(self, x: Bar): ...
        def g(self, x: Foo): ...
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:61 on 2025-03-20 11:00_

I'd feel a bit more comfortable if we used non-`self` parameters in these assertions, just because the questions "What is the type of `self`?" and "What are the legal annotations to apply to `self`?" are both somewhat complex questions. Both are irrelevant questions to the thing that's being tested here, and it's useful to make it clear that this test doesn't have any bearing on those questions, I think:

```suggestion
    def f(self, x: "Bar"):
        reveal_type(self)  # revealed: Bar

    def g(self, x: Bar):
        reveal_type(self)  # revealed: Bar
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:79 on 2025-03-20 11:01_

```suggestion
    def f(self, x: "Bar"):
        reveal_type(self)  # revealed: Bar
    # error: [unresolved-reference]
    def g(self, x: Bar):
        reveal_type(self)  # revealed: Bar
```

---

_@AlexWaygood reviewed on 2025-03-20 11:01_

Thank you! Unfortunately I'm not sure that the special-casing here is the correct approach. I think we need a more general approach to ensure that `NameError`s in annotation expressions are always detected.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:19 on 2025-03-20 22:23_

Might be less verbose to use stringified annotations in tests like these?
```suggestion
class A:
    def __add__(self, other) -> "A":
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:1217 on 2025-03-20 22:24_

I don't think this case is special enough to require a dedicated lint rule and error message. It should just result in the same undefined-reference error we would normally raise for any undefined name.

(And as discussed in another comment, that error should fall out naturally from correct handling of definitions in the semantic index.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5906 on 2025-03-20 22:48_

Yes, I agree with Alex. The bug here arises from incorrect visibility of definitions in the semantic index. Specifically, in the `ClassDef` branch of `SemanticIndexBuilder::visit_stmt`, we call `self.add_definition(...)` to record a definition for the class name, before we visit the class body. This is wrong, because it doesn't match the runtime semantics, and it causes the name to be wrongly considered defined in the class body. The fix here should simply be a matter of moving that line after we visit the class body instead.

In practice I've tried that locally and it looks like it fixes one case (self-referential inheritance) but it doesn't fix e.g. use of a class' own name in annotations of a method on that class. The reason for this appears to be a bug in our handling of eager nested scopes (cc @dcreager ). Our eager nested scope handling (see `SemanticIndexBuilder::pop_scope`) snapshots bindings, if any, but it doesn't snapshot _lack of bindings_. And then in `TypeInferenceBuilder::infer_name_load`, if we don't find any snapshotted bindings, we just fall back to looking for the name as a public symbol (that is, as we would in a reference from a lazily-executed nested scope.) So in a case like this:

```py
class A:
    def f(self) -> A:
        pass
```

Even though the class scope of `A` (which is the scope where the method annotations are evaluated) is an eager nested scope, because we don't find any bindings of `A` in the outer (global) scope at the point where `class A:` occurs, we don't snapshot any eager bindings for `A`, and then `infer_name_load`, not finding any eager bindings, wrongly falls back to looking up `A` in the global scope as a non-eager reference.

I think it would make sense to fix the two issues I describe above in two separate PRs: one for the wrong ordering in `ClassDef` visit in semantic index building, and another for fixing eager-nested-scope handling.

---

_@carljm reviewed on 2025-03-20 22:48_

---

_@mtshiba reviewed on 2025-03-22 14:35_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/infer.rs`:5906 on 2025-03-22 14:35_

OK, I'll create PRs to fix the issues you pointed out and close this PR.

---

_Closed by @carljm on 2025-03-23 14:19_

---

_Comment by @carljm on 2025-03-23 14:19_

Closed in favor of #16915 and #16916

---

_Branch deleted on 2025-03-24 14:29_

---
