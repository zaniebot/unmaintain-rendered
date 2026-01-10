```yaml
number: 14120
title: Add support for resolving metaclasses
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/meta
created_at: 2024-11-06T02:38:09Z
updated_at: 2024-11-06T21:33:12Z
url: https://github.com/astral-sh/ruff/pull/14120
synced_at: 2026-01-10T20:50:57Z
```

# Add support for resolving metaclasses

---

_Pull request opened by @charliermarsh on 2024-11-06 02:38_

## Summary

I mirrored some of the idioms that @AlexWaygood used in the MRO work.

Closes https://github.com/astral-sh/ruff/issues/14096.


---

_Review requested from @carljm by @charliermarsh on 2024-11-06 02:38_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-11-06 02:38_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-06 02:38_

---

_Review requested from @sharkdp by @charliermarsh on 2024-11-06 02:38_

---

_@charliermarsh reviewed on 2024-11-06 02:39_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:27 on 2024-11-06 02:39_

Should this be `A` rather than `Literal[A]`?

---

_Label `red-knot` added by @charliermarsh on 2024-11-06 02:40_

---

_@charliermarsh reviewed on 2024-11-06 02:44_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:2105 on 2024-11-06 02:44_

Not totally certain what to do here.

---

_Comment by @charliermarsh on 2024-11-06 03:31_

(Oops, it looks like I did this prior to pulling and need to resolve some conflicts.)

---

_Converted to draft by @charliermarsh on 2024-11-06 03:32_

---

_Marked ready for review by @charliermarsh on 2024-11-06 03:43_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:2063 on 2024-11-06 03:45_

Not totally sure what to do with these non-`Class` bases.

---

_@charliermarsh reviewed on 2024-11-06 03:45_

---

_@charliermarsh reviewed on 2024-11-06 03:46_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:27 on 2024-11-06 03:46_

(I don't feel confident about my understanding and usage of `ClassLiteralType` vs. `Class`.)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1989 on 2024-11-06 06:13_

We should add a note similar to the `Type::node` function that this function should only ever be called from queries that belong to the same file as the class.
We otherwise end up introducing cross-file query-dependencies where some queries need to re-run when the AST of another file changes -> bad for incremental performance.

---

_Comment by @carljm on 2024-11-06 06:29_

Looks like there are test failures here, and the error output assertions in the benchmark need to be updated. (I haven't looked closely at the discrepancy; it may just be that we need to update the assertion, or it may highlight an issue with the PR that should be fixed.)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2012 on 2024-11-06 06:57_

Nit: I suggest moving the shared logic for finding the keyword out of the if branch

```
let metaclass_value = class_stmt
      .keywords()
      .iter()
      .find(|keyword| keyword.arg.as_ref().is_some_and(|arg| arg == "metaclass"))
      .map(|keyword| &keyword.value);

metaclass_value.map(|base_node| {
	if class_stmt.type_params.is_some() {
		...
	} else {
		...
	}
})
```

It makes it more apparent what the difference between the two branches is. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1988 on 2024-11-06 06:59_

Nit: Move next to the `metaclass` method or next to `explicit_base_query`. The way it's ordered now requires a lot of jumping around to read the code.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2059 on 2024-11-06 07:03_

We should make this a salsa query because what I understand is that this function can also be called cross module (e.g. resolve a base class from another file and we then lookup it's base). 

See https://github.com/astral-sh/ruff/pull/14087 for a more in depth explanation. 

```suggestion
    #[salsa::tracked]
    pub(crate) fn metaclass(self, db: &'db dyn Db) -> Result<Type<'db>, MetaclassError> {
```

---

_@MichaReiser requested changes on 2024-11-06 07:04_

---

_Comment by @charliermarsh on 2024-11-06 12:11_

```python
class NotSubscriptable: ...

a = NotSubscriptable[0]  # error: "Cannot subscript object of type `Literal[NotSubscriptable]` with no `__class_getitem__` method"
```

We no longer throw this error -- I think because we resolve the metaclass to `type`, which allows subscripting (?). That seems wrong to me but I must be misunderstanding something about the semantics.

---

_Review requested from @MichaReiser by @charliermarsh on 2024-11-06 12:14_

---

_Comment by @charliermarsh on 2024-11-06 12:14_

I think the remaining failures represent a misunderstanding or lack of nuance in how we handle metaclass lookups / `__class_getitem__` so would appreciate reviews on that component.

---

_@MichaReiser reviewed on 2024-11-06 12:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2055 on 2024-11-06 12:16_

Calling this function from other files is fine, because you now added the `#[salsa::tracked`] which "isolates" the dependencies (dependent queries only re-run when the returned `Result` is different from previous run)
```suggestion
```

But this comment should be added to `explicit_metaclass`

---

_Review requested from @MichaReiser by @MichaReiser on 2024-11-06 12:33_

---

_Comment by @charliermarsh on 2024-11-06 13:30_

Gonna do a bit more research into how subscripts interact with this.

---

_Converted to draft by @charliermarsh on 2024-11-06 13:34_

---

_@AlexWaygood reviewed on 2024-11-06 13:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:27 on 2024-11-06 13:37_

This should be `Literal[A]`!

You're confusing three things here, I think. Remember that every type is a set of possible runtime values.
- All `ClassLiteralType`s are sets of size 1. The sole member of the set described by `Literal[A]` (which is a class-literal type) is the class object `A` itself.
- `InstanceType`s are sets of (usually) unknown size. The set described by the type `A` (which is an instance type) is the set of "all possible instances of `A` that could ever exist". Note that this is a disjoint set from the set described by `Literal[A]`: the class object `A` is not an instance of `A`.
- We have a struct named `Class` that is used as the inner data for both `InstanceType` and `ClassLiteralType`. But this struct doesn't represent a type at all; it's just a "bag of data" that you can query about a specific class definition at runtime.

---

_Comment by @charliermarsh on 2024-11-06 13:49_

Okay, the subscript failures were all related to a misunderstanding of literal vs. instance for class types. Fixed.

---

_Comment by @charliermarsh on 2024-11-06 13:50_

But I still have one failure in the MRO tests due to a cycle somewhere.

---

_Comment by @charliermarsh on 2024-11-06 13:55_

Ok I'm not handling cycles in metaclasses. Will debug.

---

_Comment by @AlexWaygood on 2024-11-06 14:05_

> Ok I'm not handling cycles in metaclasses. Will debug.

You'll probably have to use something like the `is_class_cyclically_defined` function we currently have in `mro.rs`. Feel free to move it to another file if you need to!

---

_Comment by @github-actions[bot] on 2024-11-06 14:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @charliermarsh on 2024-11-06 14:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:14 on 2024-11-06 14:47_

Why are you deleting all these MRO tests?!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1286 on 2024-11-06 14:52_

```suggestion
                // TODO: `type[Unknown]` would be a more precise fallback
                // (needs support for <https://docs.python.org/3/library/typing.html#the-type-of-class-objects>)
                class.metaclass(db).unwrap_or(Type::Unknown)
```

---

_@MichaReiser reviewed on 2024-11-06 14:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:1 on 2024-11-06 14:53_

Is it intentional, that you remove all these tests?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2043 on 2024-11-06 14:54_

```suggestion
            .arguments
            .find_keyword("metaclass")
```

Or maybe we should add a `find_keyword` method (or just a `metaclass` method) directly to `ruff_python_ast::StmtClassDef`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2185 on 2024-11-06 14:54_

Watcher seems misleading, IMO. I think of something like a file watcher. `SeenSet`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2188 on 2024-11-06 14:55_

What's the reason for storing `initial` explicitly over just putting it into the set?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2054 on 2024-11-06 14:56_

Huh, this looks correct! I guess it makes sense, but I guess I never thought about whether the metaclass argument would be executed in the type-parameters scope. TIL:

```pycon
>>> class Foo[T](metaclass=T): pass
... 
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    class Foo[T](metaclass=T): pass
  File "<python-input-5>", line 1, in <generic parameters of Foo>
    class Foo[T](metaclass=T): pass
TypeError: 'typing.TypeVar' object is not callable
```

---

_@MichaReiser reviewed on 2024-11-06 14:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2190 on 2024-11-06 14:57_

Nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2098 on 2024-11-06 14:58_

Is there a reference in the language spec we could also link to somewhere? Maybe in the data model?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2149 on 2024-11-06 14:59_

The fact that we have to do this in multiple places makes me think that maybe we should have a `try_metaclass` method (that returns a `Result`) and a `metaclass` method (that has a fallback metaclass type), similar to the way we have a `try_mro` method and an `mro` method. That way if we change the fallback in the future (which I think is quite likely -- see my comment above), we'll only have to change it in one place.

---

_@AlexWaygood reviewed on 2024-11-06 15:00_

---

_@charliermarsh reviewed on 2024-11-06 15:13_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:2188 on 2024-11-06 15:13_

We don't have to allocate.

---

_@charliermarsh reviewed on 2024-11-06 15:14_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:14 on 2024-11-06 15:14_

Sorry, this is my backlash against not having a way to run a single test lol. Fixed.

---

_@charliermarsh reviewed on 2024-11-06 15:15_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:2149 on 2024-11-06 15:15_

I'm not really convinced of the ergonomics of that -- I don't think we should hide the need to handle errors from callers -- but I'm fine to do it for consistency.

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:2054 on 2024-11-06 15:15_

I took this from `explicit_bases`.

---

_@charliermarsh reviewed on 2024-11-06 15:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2052 on 2024-11-06 15:26_

```suggestion
        let metaclass_node = &class_stmt
            .arguments
            .as_ref()?
            .find_keyword("metaclass")?
            .value;
        if class_stmt.type_params.is_some() {
            // when we have a specialized scope, we'll look up the inference
            // within that scope
            let model = SemanticModel::new(db, self.file(db));
            metaclass_node.ty(&model)
        } else {
            // Otherwise, we can do the lookup based on the definition scope
            let class_definition = semantic_index(db, self.file(db)).definition(class_stmt);
            definition_expression_ty(db, class_definition, metaclass_node)
        }
```

---

_@AlexWaygood reviewed on 2024-11-06 15:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:12 on 2024-11-06 17:37_

nit: in general, for tests intending to test the "happy path" of a metaclass, I'd prefer we use something that actually can work as a metaclass. Otherwise, in the future if/when we introduce a diagnostic for bad metaclasses, all these tests will need updating.
```suggestion
class A(type): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:41 on 2024-11-06 17:43_

It looks like this test differs from the below test only in that `A` here is not a valid metaclass. I think it's good to have a test for an invalid metaclass, but

1) Only the below test should be named "Linear inheritance", this test should have a name that clarifies its purpose is to test an invalid metaclass, like "Invalid metaclass"
2) The "invalid metaclass" test probably doesn't need the double layer of B and C, it could just have B; unless we are aware of some particular issue with inherited bad metaclasses that we need to test for.
3) The "invalid metaclass" test should have a TODO comment that we should emit a diagnostic for the invalid metaclass, even though we don't yet today.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:52 on 2024-11-06 17:44_

```suggestion
class A(type): ...
class B(metaclass=A): ...
class C(type): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:55 on 2024-11-06 17:48_

Nit: "compatible" is kind of a hand-wavy word, it's not clear what precisely it means. I would rather say "`Literal[C]` and `Literal[A]` have no subclass relationship"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:80 on 2024-11-06 17:53_

```suggestion
class A(type): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:91 on 2024-11-06 17:53_

```suggestion
class A(type): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:117 on 2024-11-06 17:59_

This test is kinda odd, I'm not sure it should make any difference to `D` or `E` here that their metaclass `C` itself has a custom metaclass. But it doesn't hurt to include, and maybe it does verify something important about the implementation (I haven't dug into the implementation yet.) But like the above cases, let's make the test actually valid:

```suggestion
class A(type): ...
class B(type, metaclass=A): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 18:01_

```suggestion
## Inheritance (4)

```py
class A(type): ...
class B(type, metaclass=A): ...
class C(metaclass=B): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:153 on 2024-11-06 18:02_

So we have two tests here for this kind of odd metaclass-with-a-metaclass scenario, whose value isn't entirely clear to me (the fact that a metaclass itself has a metaclass doesn't change anything about its compatibility with other metaclasses, and both of these tests are just simple linear inheritance, so no multiple base metaclasses to reconcile).

But it seems to me that we are missing tests for the scenarios that we are more likely to get wrong in the implementation where we have multiple different inherited metaclasses that _do_ have a subclass relationship and are thus compatible, and we should pick the child one. For instance a test for a case like this:

```py
class M(type): ...
class N(M): ...
class A(metaclass=M): ...
class B(metaclass=N): ...
class C(A, B): ...

reveal_type(C.__class__)  # revealed: Literal[N]
```

Or even a really complex case like this with diamond-inherited metaclasses, which actually should error:

```py
class M(type): ...
class M1(M): ...
class M2(M): ...
class M12(M1, M2): ...

class A(metaclass=M1): ...
class B(metaclass=M2): ...
class C(metaclass=M12): ...
class D(A, B, C): ...

# error: [conflicting-metaclass] "A and B have no subclass relationship"
reveal_type(D.__class__)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2047 on 2024-11-06 18:12_

Add a test for a PEP 695 generic class with a metaclass? (No need to use the type param or test anything specific to generics, just to verify that we still resolve the right type for its metaclass.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2070 on 2024-11-06 18:15_

Do we have tests exercising getting the metaclass of a cyclically-defined class?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2091 on 2024-11-06 18:25_

I think we should add a test where one of the bases is `Unknown`, but I think the current behavior here is already mostly correct; we should (mostly) effectively ignore that base when it comes to metaclass calculation. If there are other bases, we should assume the metaclass of the unknown base is compatible with anything, and if the only base is `Unknown`, we should assume its metaclass is `type`.

The reason I say "mostly"  above is that technically if we have at least one Unknown (or Any or Todo) base, then the returned metaclass should be a `type[]` type (instead of a class literal type), and should be intersected with `Any` or `Unknown` or `Todo`. So:

```
from nonexistent_module import UnknownClass  # error: [unresolved-import]

class C(UnknownClass): ...
reveal_type(C.__class__)  # revealed: type[type] & Unknown

class M(type): ...
class A(metaclass=M): ...
class B(A, UnknownClass): ...

reveal_type(B.__class__)  # revealed: type[M] | Unknown
```

This is a bit subtle, but the reason for the intersection with Unknown in these cases is that the unknown base _might_ have a metaclass that is a subclass of `type` (in the first case) or `M` (in the second case). We reflect this possibility by intersecting with Unknown. `T & Unknown` effectively means "this type may be something narrower than T, but it can't be anything not in T".

It may seem like `type[M] & Unknown` is no different from `type[M]`, because `type[M]` is already a type that includes all subclasses of M! But the difference is that `Unknown/Any` are forgiving; they always assume the most compatible type. If you ask whether `type[M]` is a subclass of `type[N]`, the answer may be "no" -- if you ask whether `type[M] & Unknown` is a subclass of `type[N]`, the answer is always yes.

But all that said, this requires a `type[]` type, which we don't have yet, and it's not high priority, so I think we should just TODO it for this PR. I'd still want to add the above tests, but the test would reveal e.g. `Literal[M]`, with a TODO comment right above the `revealed: ` line that it should be `type[M] & Unknown` instead.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2105 on 2024-11-06 18:27_

I think this is the right thing to do; it matches what happens at runtime. It would be good to have a test for this behavior, too.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2117 on 2024-11-06 18:28_

This is a fair amount of code to duplicate; wrap it up in a closure?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:69 on 2024-11-06 18:31_

```suggestion
class A(type): ...
class B(metaclass=A): ...
class C(type): ...
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:25 on 2024-11-06 18:47_

This will fail at runtime, and at some point in the future we'll want to start emitting diagnostics about that. To future-proof the test, I'd either change the example to one that will not fail at runtime (that seems to make more sense here?):

```suggestion
class A(type): ...
class B(metaclass=A): ...
```

or add a TODO comment to say that we should emit a diagnostic about it at some point:

```suggestion
class A: ...
# TODO: will fail at runtime, should emit a diagnostic here
class B(metaclass=A): ...
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:55 on 2024-11-06 18:51_

nit: it's not immediately clear to me here what "compatible" means. Maybe something like this?

```suggestion
# error: [conflicting-metaclass] "The metaclass of a derived class (`E`) must be a subclass of the metaclasses of all its bases, but `C` is not a subclass of `A` and `A` is not a subclass of `C`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:548 on 2024-11-06 18:52_

I don't think we need this TODO? I don't think we ever have plans to de-duplicate diagnostics in a way that would de-deduplicate two diagnostics that we explicitly raise separately. And I think a cyclic class definition is a cyclic class definition; I don't think there are cases where the MRO cycle detection would fail to detect a cycle but we'd detect it here.
```suggestion
                    // Cyclic class definition diagnostic will already have been emitted above in MRO calculation.
```

An interesting question here: if this is true, do we really need cycle detection implemented separately in both MRO and metaclass calculation? It seems inefficient, both in code to maintain and at runtime. Could we instead hoist the "is cyclically defined" check (which is already done as a totally separate function in MRO calculation) out into this function, and if a class is cyclically defined, we emit a diagnostic and just `specify` (this is a Salsa feature) the result of its MRO and metaclass queries as Unknown, and never try to calculate them. Then the MRO and metaclass calculation code could both take non-cyclic as an assumed invariant.

This might be a big enough change to do as a separate PR, but it seems worth doing to me, unless I'm missing something. Curious what @AlexWaygood and @MichaReiser think.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:58 on 2024-11-06 18:52_

I know the runtime error message uses the term "non-strict", but do we know what it means by that? Can we link to a definition somewhere?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1232 on 2024-11-06 18:53_

Weird, why isn't rustfmt formatting deterministic here and below?

---

_@carljm approved on 2024-11-06 18:54_

Looks great, thank you!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2083 on 2024-11-06 18:55_

Could rename this variable to `num_seen` now that the `watcher` variable was renamed to `seen`

---

_@carljm reviewed on 2024-11-06 18:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2149 on 2024-11-06 18:57_

The thing is that in this case, there's really nothing a caller can or should do about the error -- we've already emitted a diagnostic for it in `check_class_definitions`, and once we've done that, the error _shouldn't_ be explicitly handled by any other caller, it would just result in duplicate or cascading diagnostics in some form. So it's more _correct_, not just more ergonomic, to provide most callers with an API that just tells them the type of the metaclass is `Unknown`.

This is a pattern that occurs a lot in red-knot. There's an error condition -- it's somebody's responsibility to handle that error condition and emit a diagnostic. But once that's been done, the correct thing to do is flatten the error condition into a simple `Unknown` type so the rest of the world doesn't have to care about it or try to re-handle it.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:1 on 2024-11-06 18:58_

I think your logic should handle them correctly, but I'd love it if you added explicit tests for two edge cases at the root of Python's class hierarchy: `builtins.object` and `builtins.type`. `reveal_type(object.__class__)` and `reveal_type(type.__class__)` should both be `Literal[type]`

---

_@AlexWaygood reviewed on 2024-11-06 19:03_

This is really great, thank you!

---

_@carljm reviewed on 2024-11-06 19:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:58 on 2024-11-06 19:07_

"Strict subclass" is a synonym for "proper subclass" -- that is, not including yourself. So "non-strict" is just clarifying that "subclass" here is also ok with "same type"

---

_@AlexWaygood reviewed on 2024-11-06 19:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:58 on 2024-11-06 19:12_

I think adding a sentence to the prose description of the test (or even just a footnote) explaining that might be helpful

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:530 on 2024-11-06 19:24_

This means we're now iterating over all class definitions twice, when I think we can make do with a single loop. Something like this?

<details>

```rs
    fn check_class_definitions(&mut self) {
        let class_definitions = self
            .types
            .declarations
            .values()
            .filter_map(|ty| ty.into_class_literal())
            .map(|class_ty| class_ty.class);

        for class in class_definitions {
            if let Err(mro_error) = class.try_mro(self.db).as_ref() {
                match mro_error.reason() {
                    MroErrorKind::DuplicateBases(duplicates) => {
                        let base_nodes = class.node(self.db).bases();
                        for (index, duplicate) in duplicates {
                            self.diagnostics.add(
                                (&base_nodes[*index]).into(),
                                "duplicate-base",
                                format_args!("Duplicate base class `{}`", duplicate.name(self.db)),
                            );
                        }
                    }
                    MroErrorKind::CyclicClassDefinition => self.diagnostics.add(
                        class.node(self.db).into(),
                        "cyclic-class-def",
                        format_args!(
                            "Cyclic definition of `{}` or bases of `{}` (class cannot inherit from itself)",
                            class.name(self.db),
                            class.name(self.db)
                        ),
                    ),
                    MroErrorKind::InvalidBases(bases) => {
                        let base_nodes = class.node(self.db).bases();
                        for (index, base_ty) in bases {
                            self.diagnostics.add(
                                (&base_nodes[*index]).into(),
                                "invalid-base",
                                format_args!(
                                    "Invalid class base with type `{}` (all bases must be a class, `Any`, `Unknown` or `Todo`)",
                                    base_ty.display(self.db)
                                ),
                            );
                        }
                    }
                    MroErrorKind::UnresolvableMro { bases_list } => self.diagnostics.add(
                        class.node(self.db).into(),
                        "inconsistent-mro",
                        format_args!(
                            "Cannot create a consistent method resolution order (MRO) for class `{}` with bases list `[{}]`",
                            class.name(self.db),
                            bases_list.iter().map(|base| base.display(self.db)).join(", ")
                        ),
                    )
                }
            }

            if let Err(metaclass_error) = class.try_metaclass(self.db) {
                match metaclass_error.reason() {
                    MetaclassErrorKind::Conflict {
                        metaclass1,
                        metaclass2
                    } => self.diagnostics.add(
                        class.node(self.db).into(),
                        "conflicting-metaclass",
                        format_args!(
                            "The metaclass of a derived class (`{}`) must be a subclass of the metaclasses of all its bases, but `{}` and `{}` are not compatible",
                            class.name(self.db),
                            Type::ClassLiteral(*metaclass1).display(self.db),
                            Type::ClassLiteral(*metaclass2).display(self.db),
                        ),
                    ),
                    MetaclassErrorKind::CyclicDefinition => {
                        // TODO(charlie): When diagnostics are deduplicated, add a `cyclic-class-def`
                        // diagnostic, equivalent to the above.
                    }
                }
            }
        }
    }
```

</details>

The doc-comment for the function should also be updated: we do more than just check MROs in this method now

---

_@AlexWaygood reviewed on 2024-11-06 19:24_

---

_@AlexWaygood reviewed on 2024-11-06 19:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:548 on 2024-11-06 19:31_

It's definitely an interesting question where cycle detection should go. In an earlier version of my MRO PR, I had cycle-detection as a separate Salsa query, which was called by the MRO-resolution logic but was not itself part of the MRO resolution logic (my PR did this until https://github.com/astral-sh/ruff/pull/14027/commits/e1f1874e9f195c2953f155762668f362f14145aa, which changed it to the current implementation of cycle detection in MROs).

I'd vote for leaving it as @charliermarsh has it for now, though, and investigating whether we can consolidate/optimise the cycle-detection logic as a followup.

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 19:41_

Why no `type` on `C`?

---

_@charliermarsh reviewed on 2024-11-06 19:41_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 19:42_

Or, separately, why a `type` on `B`?

---

_@charliermarsh reviewed on 2024-11-06 19:42_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1232 on 2024-11-06 19:43_

(It's because it's within a macro, I think.)

---

_@charliermarsh reviewed on 2024-11-06 19:43_

---

_@charliermarsh reviewed on 2024-11-06 19:44_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1232 on 2024-11-06 19:44_

(Oh sorry -- not sure about below, but here yeah.)

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:41 on 2024-11-06 19:50_

Thanks, just a mental error.

---

_@charliermarsh reviewed on 2024-11-06 19:50_

---

_@charliermarsh reviewed on 2024-11-06 19:55_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:153 on 2024-11-06 19:55_

Thanks, these are great!

---

_@AlexWaygood reviewed on 2024-11-06 20:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:68 on 2024-11-06 20:06_

I think I would prefer for this error message to be

```suggestion
# error: [conflicting-metaclass] "The metaclass of a derived class (`E`) must be a subclass of the metaclasses of all its bases, but `C` and `A` have no subclass relationship"
```

since the error is about a lack of a subclass relationship between classes rather than a subtype relationship between types. (`Literal[A]` is how we would describe the class-literal type with only the class object `A` in it, but `A` is how we would describe the class object `A`.) You could get there with this diff:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -2123,8 +2123,8 @@ impl<'db> Class<'db> {
                 }
                 return Err(MetaclassError {
                     kind: MetaclassErrorKind::Conflict {
-                        metaclass1: candidate,
-                        metaclass2: metaclass,
+                        metaclass1: candidate.class,
+                        metaclass2: metaclass.class,
                     },
                 });
             }
@@ -2294,8 +2294,8 @@ pub(super) enum MetaclassErrorKind<'db> {
     /// The metaclass of a derived class must be a (non-strict) subclass of the metaclasses of all
     /// its bases.
     Conflict {
-        metaclass1: ClassLiteralType<'db>,
-        metaclass2: ClassLiteralType<'db>,
+        metaclass1: Class<'db>,
+        metaclass2: Class<'db>,
     },
     /// The class inherits from itself!
     ///
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 0c0991ec4..5287df8d8 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -540,8 +540,8 @@ impl<'db> TypeInferenceBuilder<'db> {
                     format_args!(
                         "The metaclass of a derived class (`{}`) must be a subclass of the metaclasses of all its bases, but `{}` and `{}` have no subclass relationship",
                         class.name(self.db),
-                        Type::ClassLiteral(*metaclass1).display(self.db),
-                        Type::ClassLiteral(*metaclass2).display(self.db),
+                        metaclass1.name(self.db),
+                        metaclass2.name(self.db),
                     ),
                 ),
                 MetaclassErrorKind::CyclicDefinition => {
```

---

_@AlexWaygood reviewed on 2024-11-06 20:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2105 on 2024-11-06 20:15_

I'm not sure I agree with this. If a non-`type`-subclass is used as the metaclass, I think it's more likely than not that `__class__` will be something completely different to the object passed as the `metaclass=` keyword:

```pycon
>>> class Foo(metaclass=lambda *args: 42): ...
... 
>>> Foo
42
>>> Foo.__class__
<class 'int'>
```

I think with the current logic we'd infer `Foo.__class__` here as being `<lambda function>` or something? But that's just incorrect.

Even if the metaclass is something that returns a class (I think the more common case, really!), it's almost certainly going to be a callable object, and the `__class__` of the created class is not going to be the same as the callable object itself.

I think honestly we don't have a clue what's going on if a non-`type`-subclass is passed as the `metaclass=` keyword, so emitting a diagnostic and inferring `Unknown` as the value of `__class__` might be better.

---

_@carljm reviewed on 2024-11-06 20:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 20:16_

`B` needs to inherit `type` (or define a custom `__new__` taking a bunch of arguments) because it is the metaclass for `C`. Otherwise it isn't a valid metaclass and will fail immediately at runtime when the class creation machinery tries to call it with a bunch of arguments at the `class C` statement and fails.

`C` doesn't need to inherit `type` because it is never used as a metaclass in this test.

---

_@AlexWaygood approved on 2024-11-06 20:17_

I have a couple of nits outstanding (I left comments), but nothing serious or blocking. This is great!

---

_@carljm reviewed on 2024-11-06 20:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 20:18_

I think perhaps some of these tests would be clearer in what they are testing if the naming scheme for classes distinguished between "metaclasses, which inherit `type`" vs "regular classes, which don't". Like in some of my examples I used `M, N, ...` for metaclasses and `A, B, ...` for regular classes.

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:142 on 2024-11-06 20:26_

@carljm -- In your example, this was `# revealed: type[type] & Unknown` -- am I missing an `Unknown` here?

---

_@charliermarsh reviewed on 2024-11-06 20:26_

---

_@charliermarsh reviewed on 2024-11-06 20:28_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 20:28_

I see, thanks.

---

_@charliermarsh reviewed on 2024-11-06 20:33_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:128 on 2024-11-06 20:33_

(Re-did all tests to match.)

---

_@carljm reviewed on 2024-11-06 20:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:142 on 2024-11-06 20:40_

Yes, but I think you should just add a TODO for it, since you also need the `type[]` type, which hasn't landed yet (and the distinction here may not matter much practically, so we can defer the TODO until/unless we find a case where it does matter)
```suggestion
# TODO should technically be `type[type] & Unknown`
reveal_type(C.__class__)  # revealed: Literal[type]
```

---

_Merged by @charliermarsh on 2024-11-06 20:41_

---

_Closed by @charliermarsh on 2024-11-06 20:41_

---

_Branch deleted on 2024-11-06 20:41_

---

_@carljm reviewed on 2024-11-06 20:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2105 on 2024-11-06 20:43_

Oh! Great point.

I think this is something we should address in the future once we have call signature checking, and what we should do here is a) verify that the metaclass is something callable which accepts the type-new arguments (otherwise emit a diagnostic), and then b) return the return type of that callable.

For now returning `Type::Todo` with a todo comment to that effect is probably best.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:548 on 2024-11-06 21:33_

https://github.com/astral-sh/ruff/issues/14141

---

_@carljm reviewed on 2024-11-06 21:33_

---
