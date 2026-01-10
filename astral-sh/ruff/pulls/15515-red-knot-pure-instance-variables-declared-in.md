```yaml
number: 15515
title: "[red-knot] Pure instance variables declared in class body"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/pure-instance-variables
created_at: 2025-01-15T21:48:28Z
updated_at: 2025-01-17T18:00:27Z
url: https://github.com/astral-sh/ruff/pull/15515
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Pure instance variables declared in class body

---

_Pull request opened by @sharkdp on 2025-01-15 21:48_

## Summary

This is a small, tentative step towards the bigger goal of understanding instance attributes. I could imagine that this might *not* be desirable as a (mergeable) first iteration, as it could potentially turn some `@Todo(instance attributes)` types into concrete types that are not correct. For example, we currently turn the *instance* attribute access to a `variable: ClassVar[str]` into `str` without raising any diagnostic (though I'm not sure if `Unknown` would be better here than `str`). So I'm also fine with keeping this open until `ClassVar` support is also implemented.

What the PR currently does is the following:

- Adds partial support for pure instance variables declared in the class body, i.e. this case:
  ```py
  class C:
      variable1: str = "a"
      variable2 = "b"

  reveal_type(C().variable1)  # str
  reveal_type(C().variable2)  # Unknown | Literal["b"]
  ```
- Adds `property` as a known class to query for `@property` decorators
- Splits up various `@Todo(instance attributes)` cases into sub-categories.

## Test Plan

Modified existing MD tests.

---

_Label `red-knot` added by @sharkdp on 2025-01-15 21:48_

---

_Comment by @github-actions[bot] on 2025-01-15 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @sharkdp on 2025-01-16 21:51_

---

_Review requested from @carljm by @sharkdp on 2025-01-16 21:51_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-16 21:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-16 21:51_

---

_@sharkdp reviewed on 2025-01-16 21:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:175 on 2025-01-16 21:53_

This is another case that was arguably better before with the `@Todo`.

---

_@sharkdp reviewed on 2025-01-16 21:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:257 on 2025-01-16 21:53_

Is it correct that we want `Unknown | Literal[1]` for the instance attribute access, but `Literal[1] for the class variable access? (above)

---

_@sharkdp reviewed on 2025-01-16 21:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:465 on 2025-01-16 21:56_

https://github.com/python/typeshed/blob/2a461a2f7931dd58df8346aca626f5bc048e8455/stdlib/builtins.pyi#L243-L246

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:257 on 2025-01-16 23:18_

No, I think we should probably add the union with `Unknown` for class attributes also; the same principles apply. (Though perhaps less strongly in practice, since mutation of class attributes is much less common -- but pyright and mypy both allow it, and we should too since it's allowed at runtime.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:175 on 2025-01-16 23:21_

This is just because we don't yet support inferring the type when we encounter a bare-ClassVar annotation? That seems pretty easy to fix, so I'm not worried about this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2308 on 2025-01-16 23:23_

This should be a TODO, no?

---

_@carljm approved on 2025-01-16 23:29_

Looks good! I don't see a problem with landing this as-is.

---

_@MichaReiser approved on 2025-01-17 08:03_

---

_@sharkdp reviewed on 2025-01-17 09:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:257 on 2025-01-17 09:26_

Ok, I added an additional test with a TODO for this case.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2308 on 2025-01-17 09:27_

Not exactly sure what you meant by that comment, but I added the following TODO comment:
```rs
// TODO: A bare `ClassVar` should rather be treated as if the symbol was not
// declared at all.
```

---

_@sharkdp reviewed on 2025-01-17 09:27_

---

_Merged by @sharkdp on 2025-01-17 09:48_

---

_Closed by @sharkdp on 2025-01-17 09:48_

---

_Branch deleted on 2025-01-17 09:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4028 on 2025-01-17 12:21_

I'm really hopeful that we can keep our special-casing of properties to a minimum if we have a solid implementation of the descriptor protocol, since they really are just descriptor instances at the end of the day. But this seems fine for now, since properties are by far the most common descriptors around, and this improves our TODO messages, making it clear why we infer bad types for various `@property` attributes for the time being

---

_@AlexWaygood reviewed on 2025-01-17 12:22_

Very nice!

---

_@carljm reviewed on 2025-01-17 18:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2308 on 2025-01-17 18:00_

Sorry, I should have spelled out what (in my mind) the TODO is. I don't think the added comment is quite right, because a bare `ClassVar` is not equivalent to "not annotated". It means "this attribute is a pure ClassVar (that is, not settable on instances) but its declared type must be inferred from the RHS and unioned with Unknown." The latter part of this is equivalent to "not annotated" but the former part is not.

---
