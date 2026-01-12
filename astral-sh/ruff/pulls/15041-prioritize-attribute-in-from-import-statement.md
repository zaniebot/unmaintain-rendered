```yaml
number: 15041
title: Prioritize attribute in from/import statement
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/more-from-imports
created_at: 2024-12-17T20:29:08Z
updated_at: 2024-12-18T05:42:00Z
url: https://github.com/astral-sh/ruff/pull/15041
synced_at: 2026-01-12T15:55:49Z
```

# Prioritize attribute in from/import statement

---

_@dcreager_

This tweaks the new semantics from #15026 a bit when a symbol could be interpreted both as an attribute and a submodule of a package.  For `from...import`, we should actually prioritize the attribute, because of how the statement itself [is implemented](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement):

> 1. check if the imported module has an attribute by that name
> 2. if not, attempt to import a submodule with that name and then check the imported module again for that attribute

---

_Review requested from @carljm by @dcreager on 2024-12-17 20:29_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-17 20:29_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-17 20:29_

---

_Review requested from @sharkdp by @dcreager on 2024-12-17 20:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2319 on 2024-12-17 20:34_

If we wanted to get really ambitious, in this case we'd go ahead with the submodule lookup and if the submodule exists we'd union its type with the attribute type, and return that.

But I don't think we need to get that ambitious unless we see it in real code.

---

_@carljm approved on 2024-12-17 20:35_

Thank you!

---

_Comment by @github-actions[bot] on 2024-12-17 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@T-256 reviewed on 2024-12-17 20:46_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:39 on 2024-12-17 20:46_

```suggestion
reveal_type(a.b)  # revealed: <module 'a.b'>
```

---

_@T-256 reviewed on 2024-12-17 20:48_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:64 on 2024-12-17 20:48_

`b` is always `Literal[42]` in there:
```pycon
>>> from a import b
>>> import a.b
>>> b
42
```
Did you mean those tests against `a.b`?
```suggestion
reveal_type(a.b)  # revealed: <module 'a.b'>
```

---

_@carljm reviewed on 2024-12-17 21:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:64 on 2024-12-17 21:03_

> `b` is always `Literal[42]` in there

Not always; not if someone else in the process has done `import a.b` first. Though we intentionally don't model this global action-at-a-distance.

This test reflects the other known limitation of our model (currently), that our treatment of "is the submodule imported (locally)" is not flow-sensitive; if `a.b` is imported in the current module, we always treat it as if it were imported first. This is clearly documented elsewhere, but I guess to avoid confusion it could be mentioned in this test too (at least to point the reader to where it is discussed.)

So it is intentional here to test the type of `b`. (Though it wouldn't hurt also test `a.b` type.)

---

_@carljm reviewed on 2024-12-17 21:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:39 on 2024-12-17 21:04_

It is intentional to test the type of `b` here. In this case, because `import a.b` occurs first, what we are testing here does match the runtime behavior.

I guess it would be fine to _also_ test the type of `a.b`.

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:64 on 2024-12-17 21:27_

> This is clearly documented elsewhere, but I guess to avoid confusion it could be mentioned in this test too (at least to point the reader to where it is discussed.)

I agree too, thanks for clarification.

---

_@T-256 reviewed on 2024-12-17 21:27_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:39 on 2024-12-17 21:36_

Added a test of `a.b` as well

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:64 on 2024-12-17 21:37_

Added a test of `a.b` as well, and added some commentary describing how this behavior differs from the interpreter

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2319 on 2024-12-17 21:37_

Added a TODO comment

---

_@dcreager reviewed on 2024-12-17 21:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2319 on 2024-12-17 21:42_

Nit on the TODO comment. If we did this, it would not be unconditional, it would only be in the case where the attribute exists but is possibly unbound. In that specific scenario, we would ideally given the union because we don't know if the attribute exists or not.
```suggestion
        if let Symbol::Type(ty, boundness) = module_ty.member(self.db, name) {
            if boundness == Boundness::PossiblyUnbound {
                // TODO: Consider loading _both_ the attribute and any submodule and unioning
                // them together if the attribute exists but is possibly-unbound.
                self.diagnostics.add_lint(
                    &POSSIBLY_UNBOUND_IMPORT,
                    AnyNodeRef::Alias(alias),
                    format_args!("Member `{name}` of module `{module_name}` is possibly unbound",),
                );
```

---

_@carljm approved on 2024-12-17 21:43_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:63 on 2024-12-17 21:48_

```suggestion
# Python would say `Literal[42]` for `b`
```
(Just repeating Alex's job here 😅)

---

_@T-256 reviewed on 2024-12-17 21:48_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2319 on 2024-12-17 21:50_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/conflicts.md`:63 on 2024-12-17 21:51_

Done

---

_@dcreager reviewed on 2024-12-17 21:51_

---

_Merged by @dcreager on 2024-12-17 21:58_

---

_Closed by @dcreager on 2024-12-17 21:58_

---

_Branch deleted on 2024-12-17 21:58_

---

_Label `red-knot` added by @dhruvmanila on 2024-12-18 05:42_

---
