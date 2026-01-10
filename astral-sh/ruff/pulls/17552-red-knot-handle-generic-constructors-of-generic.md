```yaml
number: 17552
title: "[red-knot] Handle generic constructors of generic classes"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/generic-constructor
created_at: 2025-04-22T14:01:44Z
updated_at: 2025-04-23T19:46:55Z
url: https://github.com/astral-sh/ruff/pull/17552
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Handle generic constructors of generic classes

---

_Pull request opened by @dcreager on 2025-04-22 14:01_

We now handle generic constructor methods on generic classes correctly:

```py
class C[T]:
    def __init__[S](self, t: T, s: S): ...

x = C(1, "str")
```

Here, constructing `C` requires us to infer a specialization for the generic contexts of `C` and `__init__` at the same time.

At first I thought I would need to track the full stack of nested generic contexts here (since the `[S]` context is nested within the `[T]` context). But I think this is the only way that we might need to specialize more than one generic context at once — in all other cases, a containing generic context must be specialized before we get to a nested one, and so we can just special-case this.

While we're here, we also construct the generic context for a generic function lazily, when its signature is accessed, instead of eagerly when inferring the function body.

---

_Label `red-knot` added by @dcreager on 2025-04-22 14:01_

---

_Review requested from @carljm by @dcreager on 2025-04-22 14:01_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-22 14:01_

---

_Review requested from @sharkdp by @dcreager on 2025-04-22 14:01_

---

_Comment by @github-actions[bot] on 2025-04-22 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager reviewed on 2025-04-23 15:04_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:6070 on 2025-04-23 15:04_

@sharkdp This is where I pulled function literals out into "an enum"...though I ended up not needing any new variants for this PR, so it's still technically a struct.  At the point where we need to introduce synthetic function literals, this is where they would go.  (And this struct would become one of the variants.)

I went ahead and did this separation here even though I don't need an additional variant to clearly separate the parts apply to a function literal that appears in the source, and the generic-related stuff that comes from inheriting/specializing (which would apply to synthetic function literals once we have them, too)

---

_@AlexWaygood reviewed on 2025-04-23 15:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:5918 on 2025-04-23 15:34_

if you add an `IntoIterator` implementation for `&'a mut FunctionSignature<'db>`, then you can iterate over `&mut self` directly in `set_inherited_generic_context` and `apply_specialization` rather than having to call `iter_mut()` explicitly:

```suggestion
    fn iter_mut<'a>(&'a mut self) -> std::slice::IterMut<'a, Signature<'db>> {
        match self {
            Self::Single(signature) => std::slice::from_mut(signature).iter_mut(),
            Self::Overloaded(signatures, _) => signatures.iter_mut(),
        }
    }

    fn set_inherited_generic_context(&mut self, inherited_generic_context: GenericContext<'db>) {
        for signature in self {
            signature.set_inherited_generic_context(inherited_generic_context);
        }
    }

    fn apply_specialization(&mut self, db: &'db dyn Db, specialization: Specialization<'db>) {
        for signature in self {
            signature.apply_specialization(db, specialization);
        }
    }
}

impl<'a, 'db> IntoIterator for &'a mut FunctionSignature<'db> {
    type Item = &'a mut Signature<'db>;
    type IntoIter = std::slice::IterMut<'a, Signature<'db>>;
    fn into_iter(self) -> Self::IntoIter {
        self.iter_mut()
    }
}
```

---

_@dcreager reviewed on 2025-04-23 17:53_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:6070 on 2025-04-23 17:53_

This causes a stack overflow in the ecosystem check, and I don't want to hold up this PR on debugging that, since this change is entirely orthogonal.

---

_@dcreager reviewed on 2025-04-23 17:55_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:5918 on 2025-04-23 17:55_

This goes away when I revert extracting `FunctionLiteral` into a separate type

---

_@sharkdp reviewed on 2025-04-23 18:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:6070 on 2025-04-23 18:06_

Thanks for the reference anyway! I guess we can still look at your changes in https://github.com/astral-sh/ruff/pull/17552/commits/43ce8d58f6104a38374691f27b2aafea0eed2ca1 if we want to go back.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6088 on 2025-04-23 18:45_

Reasonable to debug-assert here that it's `None`, or is this not an invariant in this case?

---

_@carljm approved on 2025-04-23 18:51_

Looks great! No notes :) (edit: "narrator: he had one note")

---

_@dcreager reviewed on 2025-04-23 19:03_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:6088 on 2025-04-23 19:03_

I don't think it's an invariant, since then a user could trigger a panic by applying `dataclass_transform` twice.  That could be a diagnostic we want to enforce, but shouldn't be an assertion.  (Inheriting a generic context at most once is a proper invariant, since it's not possible for a user to declare a function "doubly" generic)

---

_Merged by @dcreager on 2025-04-23 19:06_

---

_Closed by @dcreager on 2025-04-23 19:06_

---

_Branch deleted on 2025-04-23 19:06_

---

_@sharkdp reviewed on 2025-04-23 19:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:429 on 2025-04-23 19:46_

Thank you

---
