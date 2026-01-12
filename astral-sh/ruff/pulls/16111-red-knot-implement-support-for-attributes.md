```yaml
number: 16111
title: "[red-knot] Implement support for attributes implicitly declared via their parameter types"
type: pull_request
state: closed
author: mishamsk
labels:
  - ty
assignees: []
base: main
head: 15960-implement-attributes-implicitly-declared-via-param-types
created_at: 2025-02-12T03:14:18Z
updated_at: 2025-03-30T20:46:03Z
url: https://github.com/astral-sh/ruff/pull/16111
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Implement support for attributes implicitly declared via their parameter types

---

_@mishamsk_

## Summary

As per discussion in #15960 - implemented a concrete special case of implicit declaration.

## Test Plan

cargo nextest run -p red_knot_python_semantic --no-fail-fast


---

_Review requested from @carljm by @mishamsk on 2025-02-12 03:14_

---

_Review requested from @MichaReiser by @mishamsk on 2025-02-12 03:14_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-02-12 03:14_

---

_Review requested from @sharkdp by @mishamsk on 2025-02-12 03:14_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-12 07:59_

---

_@sharkdp reviewed on 2025-02-12 08:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:49 on 2025-02-12 08:01_

Thank you for adding this test case. I think we could probably move it directly below the `reveal_type(c_instance.inferred_from_param)` above (and similarly move the definition in `__init__` upwards). And add a small comment that explains why we union with `Unknown` in that case.

---

_@sharkdp reviewed on 2025-02-12 08:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:26 on 2025-02-12 08:02_

Minor:

Maybe something like this for a shorter and more realistic example?
```suggestion
        param = param if param is not None else 0
```

---

_@sharkdp reviewed on 2025-02-12 08:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:51 on 2025-02-12 08:10_

This test case also makes sense, but we already have that below in the "Variable defined in non-`__init__` method" section. Notice that it comes with a TODO that shows that we want to infer the declared parameter type for these cases as well. I think it would be surprising to special-case `__init__` in the way you have done here in this PR, although I understand the intention.

So to summarize: I think we can remove this test case here; remove the `if name.as_str() == "__init__"` check in your implementation, and then resolve the TODO in the test case below.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4251 on 2025-02-12 08:14_

Maybe
```suggestion
                    // If we see that a *declared* parameter of a method is directly assigned
                    // to an instance attribute, we treat that as if the attribute assignment had
                    // been annotated with that same parameter type. For example, we treat
                    // the following attribute assignment, as if it had been annotated with
                    // `self.name: str = name`:
                    // ```python
                    // class A:
                    //     def __init__(self, name: str):
                    //         self.name = name
                    // ```
```

---

_@sharkdp reviewed on 2025-02-12 08:14_

---

_@MichaReiser reviewed on 2025-02-12 08:39_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4279 on 2025-02-12 08:39_

We'll need to wrap this in a salsa query because it access both `kind` and `node`, which both are AST nodes from other modules. 


---

_@sharkdp reviewed on 2025-02-12 08:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4277 on 2025-02-12 08:40_

Unfortunately, I don't think that this is sufficient.

In the attribute assignment `self.name = value`, the inferred type for `value` might be different than the declared type (e.g. due to narrowing). For example, consider:
```py
class C:
    def __init__(self, x: str | None = None) -> None:
        if x is not None:
            self.x = x

reveal_type(C().x)
```
On this branch, we would get the answer `str`. This seems fine for the given example, but if we extend that a bit:
```py
class C:
    def __init__(self, x: str | None = None) -> None:
        if x is not None:
            self.x = x
        else:
            self.x = 0

reveal_type(C().x)
```
we would *still* get `str` due to the early return just below, but this is now wrong (too narrow).

I think we should introduce a new section in the `attributes.md` test suite with a few test cases like the ones above.

To fix this, we might want to retrieve the declared type for the parameter, compare it with the inferred type of the RHS of the assignment, and only trigger this special case if both are equal? This would lead to the answers `Unknown | str` and `Unknown | str | Literal[0]` for these examples, which seems good to me.

---

_@sharkdp reviewed on 2025-02-12 08:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4277 on 2025-02-12 08:44_

Another test case that we should probably add is the following, to make sure that we infer `str` and not `Literal[""]`:
```py
class C:
    def __init__(self, param: str = "") -> None:
        self.x = param

reveal_type(C().x)
```

---

_@carljm reviewed on 2025-02-12 16:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4277 on 2025-02-12 16:28_

For the latter case, the inferred and declared type of `param` will both be `str` anyway. Defaults do not narrow the inferred type, since we can't assume the default is used for any given call.

(Not that it's a problem to add that test, but I don't think that one would fail even if we looked only at inferred type.)

---

_Comment by @sharkdp on 2025-02-13 12:15_

As discussed privately, we decided to postpone this feature for now. Thank you very much for your contribution. It's possible that we pick this up again if we eventually decide that we need this after all.

---

_Closed by @sharkdp on 2025-02-13 12:15_

---

_Branch deleted on 2025-03-30 20:46_

---
