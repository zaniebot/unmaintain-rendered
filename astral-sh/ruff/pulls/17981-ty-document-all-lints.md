```yaml
number: 17981
title: "[ty] Document all lints"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: ty-lints
created_at: 2025-05-09T13:19:01Z
updated_at: 2025-05-17T20:05:41Z
url: https://github.com/astral-sh/ruff/pull/17981
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Document all lints

---

_Pull request opened by @InSyncWithFoo on 2025-05-09 13:19_

## Summary

Resolves #14889/[#226](https://github.com/astral-sh/ty/issues/226).

Most lints now have their "What it does", "Why is this bad?" and "Examples" filled in.

## Test Plan

No test plan.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-09 13:19_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-09 13:19_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-09 13:19_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-09 13:19_

---

_Comment by @MichaReiser on 2025-05-09 13:23_

@AlexWaygood could you own the review of this PR?

---

_Comment by @github-actions[bot] on 2025-05-09 13:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-09 13:25_

---

_@AlexWaygood reviewed on 2025-05-09 13:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:117 on 2025-05-09 13:35_

this is a good example but it brings attention to the fact that the `## What it does` and `## Why is this bad?` sections that were already here are subtly incorrect. It looks like we only emit this diagnostic on _implicit_ calls to _dunder_ methods. If you try to do an _explicit_ call to a _non-dunder_ method, then we just use the `possibly-unbound-attribute` diagnostic.

That all makes sense to me, but I think to make this clearer we should:
- Rename the lint to `POSSIBLY_UNBOUND_DUNDER_CALL`
- change the `summary` field of the lint, and the "What it does" section of the documentation here, to "detects implicit calls to possibly unbound dunder methods"
- Add a sentence of explanation to the "Why is this bad?" section of the documentation that says that syntax such as `x[y]` or `x ** y` implicitly calls dunder methods under the hood

---

_@AlexWaygood reviewed on 2025-05-09 13:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:117 on 2025-05-09 13:35_

Maybe leave this one for now and tackle all this in a followup PR? All ^that feels like enough for one PR on its own

---

_@AlexWaygood reviewed on 2025-05-09 13:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:120 on 2025-05-09 13:36_

Can we add (or link to) a definition of "type form" somewhere here? I think most users will be unfamiliar with the term

---

_@AlexWaygood reviewed on 2025-05-09 13:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:142 on 2025-05-09 13:38_

I find the `reveal_type` call distracting here

```suggestion
    /// f(int)  # error
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:202 on 2025-05-09 13:42_

```suggestion
    /// Checks for class definitions in stub files that inherit (directly or indirectly)
    /// from themselves.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:213 on 2025-05-09 13:44_

```suggestion
    /// Although forward references are natively supported in stub files,
    /// inheritance cycles are still disallowed, as it is impossible to
    /// resolve a consistent [method resolution order] for a class that
    /// inherits from itself.
    ///
    /// ## Examples
    /// ```python
    /// class A(B): ...
    /// class B(A): ...
    /// ```
    ///
    /// [method resolution order]: https://docs.python.org/3/glossary.html#term-method-resolution-order
```

---

_@AlexWaygood reviewed on 2025-05-09 13:44_

---

_@AlexWaygood reviewed on 2025-05-09 13:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:178 on 2025-05-09 13:46_

this isn't accurate -- two classes can be used in multiple inheritance together fine even if they have different metaclasses, as long as one metaclass is a subclass of the other:

```pycon
>>> class M1(type): ...
... 
>>> class M2(M1): ...
... 
>>> class Foo(metaclass=M1): ...
... 
>>> class Bar(metaclass=M2): ...
... 
>>> class Baz(Foo, Bar): ...
...
>>> 
```

---

_@AlexWaygood reviewed on 2025-05-09 13:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:357 on 2025-05-09 13:49_

the example is great. Could we also add a link to https://docs.python.org/3/glossary.html#term-method-resolution-order at the first mention of `MRO` in the "What it does" section?

---

_@AlexWaygood reviewed on 2025-05-09 13:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:439 on 2025-05-09 13:52_

```suggestion
    /// is not [assignable to] the type of the assignee.
    ///
    /// ## Why is this bad?
    /// Such assignments break the rules of the type system and weaken a type checker's
    /// ability to accurately reason about your code.
    ///
    /// ## Examples
    /// ```python
    /// a: int = ''
    /// ```
    ///
    /// [assignable to]: https://typing.python.org/en/latest/spec/glossary.html#term-assignable
```

---

_@AlexWaygood reviewed on 2025-05-09 13:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:459 on 2025-05-09 13:57_

```suggestion
    /// Checks for classes that have bases which are not themselves
    /// either a class or a [gradual form]
    ///
    /// ## Why is this bad?
    /// Many such class definitions will lead to `TypeError` being raised at runtime.
    ///
    /// ## Examples
    /// ```python
    /// class A(1): ...
    /// class B(A()): ...
    /// ```
    ///
    /// [gradual form]: https://typing.python.org/en/latest/spec/glossary.html#term-gradual-form
```

We should also probably break this diagnostic up into two. The first of these classes will fail at runtime, but the second one won't -- it just can't be properly understood by ty: https://play.ty.dev/5acf6092-7608-4c4f-8e73-989ea03ab029.

We're using the same error code for both class definitions; that seems inappropriate. 

---

_@AlexWaygood reviewed on 2025-05-09 14:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:500 on 2025-05-09 14:02_

```suggestion
    /// Checks for declarations where the inferred type of an existing symbol
    /// is not [assignable to] its post-hoc declared type.
    ///
    /// ## Why is this bad?
    /// Such declarations break the rules of the type system and weaken a type checker's
    /// ability to accurately reason about your code.
    ///
    /// ## Examples
    /// ```python
    /// a = 1
    /// a: str
    /// ```
    ///
    /// [assignable to]: https://typing.python.org/en/latest/spec/glossary.html#term-assignable
```

---

_@AlexWaygood reviewed on 2025-05-09 14:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:673 on 2025-05-09 14:03_

```suggestion
    /// This breaks the rules of the type system and weakens a type checker's
    /// ability to accurately reason about your code.
```

---

_@AlexWaygood reviewed on 2025-05-09 14:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:795 on 2025-05-09 14:04_

```suggestion
    /// but cannot validly be interpreted as such.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:817 on 2025-05-09 14:04_

```suggestion
    /// Checks for constrained type variables with only one constraint.
```

Can we also add a link to https://docs.python.org/3/library/typing.html#typing.TypeVar, which has a nice explanation of the difference between bound and constrained typevars

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:821 on 2025-05-09 14:05_

```suggestion
    /// A constrained type variable must have at least two constraints.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:834 on 2025-05-09 14:12_

```suggestion
    /// T = TypeVar('T', str)  # invalid constrained TypeVar
    /// ```
    ///
    /// Use instead:
    /// ```python
    /// T = TypeVar('T', str, int)  # valid constrained TypeVar
    /// # or
    /// T = TypeVar('T', bound=str)  # valid bound TypeVar
```

---

_@AlexWaygood reviewed on 2025-05-09 14:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:988 on 2025-05-09 14:14_

We've floated the idea in the past of not reporting this diagnostic at all for instance attributes. Let's make this example show an attribute access on the class object itself, so that it will still be valid even if we make that change:

```suggestion
    /// A.c  # AttributeError: type object 'A' has no attribute 'c'
```

---

_@AlexWaygood reviewed on 2025-05-09 14:14_

---

_Comment by @MichaReiser on 2025-05-09 14:17_

@InSyncWithFoo will you be able to address the comments today. If not, @ntBre could you pick up the PR.

---

_@AlexWaygood reviewed on 2025-05-09 14:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1009 on 2025-05-09 14:19_

`b` is undefined here -- and we could also make this a bit more fun:

```suggestion
    /// import datetime
    ///
    /// if datetime.date.today().weekday() != 6:
    ///     a = 1
```

---

_@AlexWaygood reviewed on 2025-05-09 14:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1214 on 2025-05-09 14:21_

```suggestion
    /// Importing a module that cannot be resolved will raise a `ModuleNotFoundError`
```

---

_@AlexWaygood reviewed on 2025-05-09 14:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1293 on 2025-05-09 14:23_

```suggestion
    /// A `static_assert` call represents an explicit request from the user
    /// for the type checker to emit an error if the argument cannot be verified
    /// to evaluate to `True` in a boolean context.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1315 on 2025-05-09 14:24_

hmm, not necessarily. In the example given immediately below, the assignment won't raise `AttributeError`. It's just that the user has explicitly stated that assignments on instances should be prohibited by annotating the variable with `ClassVar`.

---

_@AlexWaygood reviewed on 2025-05-09 14:24_

---

_@InSyncWithFoo reviewed on 2025-05-09 14:54_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/diagnostic.rs`:120 on 2025-05-09 14:54_

"Type form" doesn't exist in [the glossary](https://typing.python.org/en/latest/spec/glossary.html) yet. I think it was popularized by this statement in [PEP 747](https://peps.python.org/pep-0747/):

> [Type expressions](https://typing.python.org/en/latest/spec/annotations.html#type-expression) provide a standardized way to specify types in the Python type system. When a type expression is evaluated at runtime, the resulting <i>type form object</i> encodes the information supplied in the type expression.

The PEP is still in draft though. Also, as per this definition, a <i>type form object</i> is an object that is the result of a type expression at runtime (be it a string, a `UnionType`, etc.). I think our use of "type form" slightly differs from that.

---

_Comment by @ntBre on 2025-05-09 15:12_

> @InSyncWithFoo will you be able to address the comments today. If not, @ntBre could you pick up the PR.

Happy to pick this up, just let me know!

---

_Comment by @InSyncWithFoo on 2025-05-09 15:24_

> will you be able to address the comments today.

Sure, no sweat. @ntBre Thanks for offering, but I can handle this one.

---

_@InSyncWithFoo reviewed on 2025-05-09 15:29_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/diagnostic.rs`:459 on 2025-05-09 15:29_

Just to be clear, these are all allowed, correct?

```python
def f(a: Any, b: Unknown, c: list[int]):  # `c: Todo`
    class A(a): ...
    class B(b): ...
    class C(c): ...

    class D(Any): ...
    class E(Unknown): ...
```

If so, then I think the description should be:

> Checks for class bases which are not themselves a class, a [gradual form](https://typing.python.org/en/latest/spec/glossary.html#term-gradual-form) (`typing.Any`, `ty_extensions.Unknown`) or of a gradual type (`Any`, `Unknown`, `Todo`).

---

_@AlexWaygood reviewed on 2025-05-09 15:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:459 on 2025-05-09 15:40_

> Just to be clear, these are all allowed, correct?

oh, great point! I still think "of a gradual type" isn't quite right, since according to [the spec](https://typing.python.org/en/latest/spec/glossary.html#term-gradual-type):

> All types in the Python type system are “gradual”

This conversation is just further convincing me that we should split this lint into two, because there are many types which we currently reject here that are assignable to `type` (and therefore should be valid bases, since they (probably) won't lead to `TypeError` being raised at runtime), but which are disallowed by this lint because we wouldn't be able to infer what the MRO of the resulting class would be. It's two distinct problems that have two distinct consequences, but they're currently covered by the same error code ☹️

Feel free to leave the documentation of this lint for now. I'm not sure it makes sense to add documentation when the lint itself doesn't really feel like it makes sense right now.

---

_@AlexWaygood reviewed on 2025-05-09 16:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:178 on 2025-05-09 16:04_

```suggestion
    /// Checks for class definitions where the metaclass of the class
    /// being created would not be a subclass of the metaclasses of
    /// all the class's bases.
```

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-05-09 16:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:120 on 2025-05-09 16:42_

Yeah. You could maybe switch to using "type expression" instead of "type form"? That is well defined -- you could link to https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions

---

_@AlexWaygood reviewed on 2025-05-09 16:42_

---

_@AlexWaygood approved on 2025-05-09 16:42_

This is great. Thanks so much for doing this, and for addressing the review comments so quickly! Will land as soon as https://github.com/astral-sh/ruff/pull/17981/files#r2081715932 is resolved

---

_@InSyncWithFoo reviewed on 2025-05-09 16:56_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/diagnostic.rs`:120 on 2025-05-09 16:56_

Actually, I think this lint's documentation is using "type form" correctly (the one that should be changed is `invalid-type-form`). "Type expression" would be wrong in this context, even.

---

_@AlexWaygood reviewed on 2025-05-09 17:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:120 on 2025-05-09 17:06_

Okay, let's leave this for now

---

_Merged by @AlexWaygood on 2025-05-09 17:06_

---

_Closed by @AlexWaygood on 2025-05-09 17:06_

---

_Branch deleted on 2025-05-17 20:05_

---
