```yaml
number: 13576
title: Add diagnostic for integer division by zero
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/rk-int-div-zero
created_at: 2024-09-30T21:00:53Z
updated_at: 2024-10-01T10:31:26Z
url: https://github.com/astral-sh/ruff/pull/13576
synced_at: 2026-01-12T15:55:44Z
```

# Add diagnostic for integer division by zero

---

_@zanieb_

Adds a diagnostic for division by the integer zero in `//`, `/`, and `%`.

Doesn't handle `<int> / 0.0` because we don't track the values of float literals.

---

_Review requested from @carljm by @zanieb on 2024-09-30 21:00_

---

_Review requested from @MichaReiser by @zanieb on 2024-09-30 21:00_

---

_Review requested from @AlexWaygood by @zanieb on 2024-09-30 21:00_

---

_Label `red-knot` added by @zanieb on 2024-09-30 21:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2301 on 2024-09-30 21:11_

I'd remove this TODO as it's not clear that we ever need to do this.

(Interestingly, it looks like neither mypy nor pyright consider division by zero for integers to be a type error. I think we should, since we can, but I don't think this feature alone justifies adding a float literal type.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2308 on 2024-09-30 21:13_

Not sure if we should infer `Unknown` here, or go ahead and infer `builtins_symbol_ty(db, "float")` (or int for FloorDiv) despite the error? No other type would be reasonable for checking subsequent code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2318 on 2024-09-30 21:14_

```suggestion
                // It should be impossible to hit this as we handle division by zero above and
```

---

_Comment by @github-actions[bot] on 2024-09-30 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-09-30 21:15_

I'd be fine with turning this into an `expect` -- if the comment above is wrong, I'd rather find out loudly so we can consider that case, rather than just silently get Unknown.

---

_@zanieb reviewed on 2024-09-30 21:16_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:4212 on 2024-09-30 21:16_

We could want a special message for modulo.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4178 on 2024-09-30 21:16_

I would rather have this as a separate test case function.

Though since it will all change with test framework anyway, it's OK to not bother.

---

_@carljm approved on 2024-09-30 21:16_

---

_@zanieb reviewed on 2024-09-30 21:22_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2301 on 2024-09-30 21:22_

We can support a diagnostic for `1.0 / 0` today without tracking float literals though, right?

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2308 on 2024-09-30 21:23_

I'm not sure. I defer to you. It seems fairly reasonable either way, though not using `Unknown` lets us check more code in a single invocation.

---

_@zanieb reviewed on 2024-09-30 21:23_

---

_@carljm reviewed on 2024-09-30 21:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2301 on 2024-09-30 21:25_

Oh true, I guess I didn't know which this todo referred to. And that reminds me, I forgot to reply to your comment about handling non-literal types. Will do that.

---

_@carljm reviewed on 2024-09-30 21:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2308 on 2024-09-30 21:26_

Yeah, I think I'd favor inferring float or int rather than Unknown here.

---

_Comment by @carljm on 2024-09-30 21:27_

> Doesn't handle `<float> / 0` because it's not clear to me what the proper pattern is to match that type. I presume I would check for equality after looking up the builtin?

I think it should be something like a match pattern for `Type::Instance(cls) if cls.is_stdlib_symbol(db, "builtins", "float")` to match the float type; similar for int.

---

_@zanieb reviewed on 2024-09-30 21:55_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2322 on 2024-09-30 21:55_

I did some refactoring here in light of the desire for retaining the return types as in https://github.com/astral-sh/ruff/pull/13576#discussion_r1781740580

---

_Review requested from @carljm by @zanieb on 2024-09-30 22:14_

---

_Comment by @zanieb on 2024-09-30 22:14_

I made some relatively significant changes per the review. Thanks again Carl.

---

_@zanieb reviewed on 2024-09-30 22:20_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:2342 on 2024-09-30 22:20_

Per discussion, division by zero (and overflows, which should never occur here) don't matter when determining the inferred type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:520 on 2024-09-30 22:25_

For @AlexWaygood ;)

```suggestion
            format_args!("Cannot divide object of type '{}' by zero.", left.display(self.db),),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4212 on 2024-09-30 22:27_

Modulo requires division to calculate, so I think the error message is accurate and adequate as is.

---

_@carljm approved on 2024-09-30 22:27_

---

_@zanieb reviewed on 2024-09-30 22:34_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:4212 on 2024-09-30 22:34_

Agree, but not all beginners may realize that! Seems fine unless we receive significant feedback though.

---

_Merged by @zanieb on 2024-09-30 22:38_

---

_Closed by @zanieb on 2024-09-30 22:38_

---

_Branch deleted on 2024-09-30 22:38_

---

_@AlexWaygood reviewed on 2024-10-01 10:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4212 on 2024-10-01 10:31_

Yeah I agree with @zanieb here -- the current message is entirely accurate, but I think even your _average_ Python user (letalone a beginner) doesn't necessarily think about modulo operations in terms of their desugared sub-operations, so I think encountering this error message here might be pretty surprising to a lot of our users. It's definitely not the biggest issue in the world, but I think a better error message here would be really worthwhile (and I actually think it's probably better to do it now before we forget about it)

---
