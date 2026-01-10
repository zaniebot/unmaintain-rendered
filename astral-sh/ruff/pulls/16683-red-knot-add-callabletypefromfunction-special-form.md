```yaml
number: 16683
title: "[red-knot] Add `CallableTypeFromFunction` special form"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-type-from-function
created_at: 2025-03-12T16:09:36Z
updated_at: 2025-03-13T02:19:36Z
url: https://github.com/astral-sh/ruff/pull/16683
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Add `CallableTypeFromFunction` special form

---

_Pull request opened by @dhruvmanila on 2025-03-12 16:09_

## Summary

This PR adds a new `CallableTypeFromFunction` special form to allow extracting the abstract signature of a function literal i.e., convert a `Type::Function` into a `Type::Callable` (`CallableType::General`).

This is done to support testing the `is_gradual_equivalent_to` type relation specifically the case we want to make sure that a function that has parameters with no annotations and does not have a return type annotation is gradual equivalent to `Callable[[Any, Any, ...], Any]` where the number of parameters should match between the function literal and callable type.

Refer https://github.com/astral-sh/ruff/pull/16634#discussion_r1989976692

### Bikeshedding

The name `CallableTypeFromFunction` is a bit too verbose. A possibly alternative from Carl is `CallableTypeOf` but that would be similar to `TypeOf` albeit with a limitation that the former only accepts function literal types and errors on other types.

Some other alternatives:
* `FunctionSignature`
* `SignatureOf` (similar issues as `TypeOf`?)
* ...

## Test Plan

Update `type_api.md` with a new section that tests this special form, both invalid and valid forms.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-12 16:09_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-12 16:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-12 16:09_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-12 16:09_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-12 16:09_

---

_Comment by @github-actions[bot] on 2025-03-12 16:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @sharkdp on 2025-03-12 18:07_

More of a high-level comment. I'm not completely convinced that we need a special form in `knot_extensions` for this. It seems to me like something that could be implemented with core type-system features, once we support generics?
```py
def signature_of[*Ts, R](c: Callable[[*Ts], R]) -> Callable[[*Ts], R]:
    return c
```

which could be used like [this](https://mypy-play.net/?mypy=latest&python=3.13&gist=ebd4293f4d70eeed7083577243097ac7).

---

_Comment by @carljm on 2025-03-12 20:00_

> something that could be implemented with core type-system features, once we support generics

This approach can still only express callable types that can be spelled with `Callable` syntax, which is quite limiting (no named/keyword parameters, no optional/defaulted parameters, no omitted annotations, no overloads...). I think we need this special form because our callable type is capable of expressing many signatures that cannot be spelled using core type forms.

I think a more promising way to (partially?) obviate the need for this special form is "callable Protocols" (that is, a Protocol that defines only a `__call__` method). But in this case, I'm not sure yet (until we implement protocols) whether we would special-case interpreting that Protocol as a general callable type, or whether it would retain a distinct representation as a Protocol but implement type equivalency with the equivalent callable type. (Or, possibly, we could even transition "general callable type" to simply be one kind of Protocol. Similarly, `AlwaysTruthy` and `AlwaysFalsy` _could_ be represented as a `Protocol` with `bool` method returning `Literal[True]` or `Literal[False]`.)

Given the simplicity of this special form, and the fact that we can use it today to help us test callable type relations more thoroughly, I'm still in favor of landing it. We can always consider removing it later (I guess barring the possibility that it sees wide external adoption, but this is unlikely since it doesn't exist at runtime, and if it does see wide adoption despite that, it suggests that it is useful and shouldn't be removed.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4305 on 2025-03-12 20:03_

We may want our general callable type to support overloaded signatures? But this can be left as a future discussion.

---

_@carljm approved on 2025-03-12 20:03_

---

_@dhruvmanila reviewed on 2025-03-13 02:12_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4305 on 2025-03-13 02:12_

Yeah, I mainly meant this as a TODO, will add it there.

---

_Comment by @dhruvmanila on 2025-03-13 02:15_

Additionally, this will also be useful to test out assignability because `lambda` expressions are limited in that sense. Protocols would be the best in that scenarios but this would at least allow us to write tests for all possible forms of `Callable` annotations along against various function signatures.

---

_Merged by @dhruvmanila on 2025-03-13 02:19_

---

_Closed by @dhruvmanila on 2025-03-13 02:19_

---

_Branch deleted on 2025-03-13 02:19_

---
