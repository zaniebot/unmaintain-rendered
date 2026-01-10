```yaml
number: 13590
title: "Implement arithmetic inference for binary expressions with `float` and `int` instances"
type: pull_request
state: closed
author: zanieb
labels:
  - ty
assignees: []
draft: true
base: main
head: zb/rk-float-int
created_at: 2024-10-01T14:57:08Z
updated_at: 2024-10-19T15:23:38Z
url: https://github.com/astral-sh/ruff/pull/13590
synced_at: 2026-01-10T20:59:36Z
```

# Implement arithmetic inference for binary expressions with `float` and `int` instances

---

_Pull request opened by @zanieb on 2024-10-01 14:57_

Following #13576 adds more inference for binary expressions including `float` and `int` instances rather than just `IntLiteral`

---

_Label `red-knot` added by @zanieb on 2024-10-01 14:57_

---

_@zanieb reviewed on 2024-10-01 14:59_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 14:59_

I might futz with the organization of these and add some comments?

---

_@zanieb reviewed on 2024-10-01 15:13_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 15:13_

cc @carljm regarding the handling of `IntLiteral` without it being a special case

---

_@AlexWaygood reviewed on 2024-10-01 16:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 16:17_

I think we probably want a general-purpose function like this for binary operations between instance types:

```rs
fn perform_binary_operation<'db>(
    db: &'db dyn Db,
    left: ClassType<'db>,
    right: ClassType<'db>,
    dunder_name: &str,
) -> Option<Type<'db>> {
    // TODO the reflected dunder actually has priority if the r.h.s. is a strict subclass of the l.h.s.
    // TODO Some other complications too!

    let dunder = left.class_member(db, dunder_name);
    if !dunder.is_unbound() {
        return dunder
            .call(db, &[Type::Instance(left), Type::Instance(right)])
            .return_ty(db);
    }

    let reflected_dunder = right.class_member(db, &format!("r{dunder_name}"));
    if !reflected_dunder.is_unbound() {
        return dunder
            .call(db, &[Type::Instance(right), Type::Instance(left)])
            .return_ty(db);
    }

    None
}
```

And then for e.g. inferring `x * y`, where `x` is an instance type and `y` is an `IntLiteral` type we'd just do something like

```rs
let Type::Instance(x_class) = x;
let int_class = builtins_symbol_ty(&db, "int");
perform_binary_operation(db, x_class, int_class, "__mul__")
```

---

_@AlexWaygood reviewed on 2024-10-01 16:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 16:20_

What `perform_binary_operation` when passed `"__mul__"` does is:
- Lookup the `__mul__` function on the type of `x`
- If it exists, call `__mul__(x, y)`
- If `type(x).__mul__` did not exist, lookup `__rmul__` on the type of `y`
- If `type(y).__rmul__` exists, call `__rmul__(y, x)`
- Else, return `None`

This is a generalised routine that will work for inferring binary operations between any two instance types. And unless we have a literal on both sides of the binary operation, we may as well treat an `IntLiteral` variant as "just an instance of `int`", and fallback to the generalised routine

---

_@AlexWaygood reviewed on 2024-10-01 16:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 16:22_

The algorithm for inferring binary operations is unfortunately really really complicated in its totality, though. See https://snarky.ca/unravelling-binary-arithmetic-operations-in-python/ for all the gory details!

---

_@zanieb reviewed on 2024-10-01 16:29_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 16:29_

Interesting! Thanks for the context.

---

_@carljm reviewed on 2024-10-01 17:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 17:44_

Yep, @AlexWaygood perfectly summarized what I was talking about in standup, and with lots more useful details, too!!

---

_@carljm reviewed on 2024-10-01 17:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 17:48_

Yeah, so either way we are going to have large match statements, but once we are in the realm of "things typeshed can tell us" (which is all of the cases added in this PR), we should avoid adding new special case match arms and just have a generic version that looks up and "calls" the appropriate dunder methods like Alex suggests. Pretty much we should only have special-case implementations for literal types if there is a possibility we can infer a literal type out of the operation (something typeshed clearly can't do), otherwise we should be falling back to the general case.

---

_@zanieb reviewed on 2024-10-01 17:49_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 17:49_

Makes sense, I was thinking something was wrong here but wasn't aware typeshed does all of this.

---

_@AlexWaygood reviewed on 2024-10-01 18:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2410 on 2024-10-01 18:08_

Oh yeah, we go to great lengths to accurately reflect all the dunders in typeshed: <https://github.com/python/typeshed/blob/44aa63330b03bdacab731af2333ff9bf70855de3/stdlib/builtins.pyi#L227-L330>

And we have quite extensive testing to check that none are omitted from the stub or have inconsistent signatures with what actually exists at runtime.

---

_Closed by @AlexWaygood on 2024-10-19 15:22_

---

_Branch deleted on 2024-10-19 15:23_

---
