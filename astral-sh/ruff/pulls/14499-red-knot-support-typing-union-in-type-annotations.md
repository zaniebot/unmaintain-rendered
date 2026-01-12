```yaml
number: 14499
title: "[red-knot] support `typing.Union` in type annotations"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-typing-union
created_at: 2024-11-20T20:12:09Z
updated_at: 2025-05-21T20:26:45Z
url: https://github.com/astral-sh/ruff/pull/14499
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] support `typing.Union` in type annotations

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Fix #14498

## Summary

This PR adds `typing.Union` support

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

I created new tests in mdtest.


---

_Review requested from @carljm by @Glyphack on 2024-11-20 20:12_

---

_Review requested from @MichaReiser by @Glyphack on 2024-11-20 20:12_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-11-20 20:12_

---

_Review requested from @sharkdp by @Glyphack on 2024-11-20 20:12_

---

_Label `red-knot` added by @MichaReiser on 2024-11-20 20:14_

---

_Comment by @Glyphack on 2024-11-20 20:15_

Is it possible to auto tag my PR as redknot by using a specific word in the description or title?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4574 on 2024-11-20 20:16_

Dummy question: Is it required to use the builder when we infer a single type? Can't we just return the type itself?

---

_Renamed from "Redknot typing union" to "[red-knot] support `typing.Union` in type annotations" by @Glyphack on 2024-11-20 20:16_

---

_@MichaReiser reviewed on 2024-11-20 20:16_

---

_Converted to draft by @Glyphack on 2024-11-20 20:22_

---

_Comment by @github-actions[bot] on 2024-11-20 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Glyphack reviewed on 2024-11-20 20:27_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4574 on 2024-11-20 20:27_

It has some extra logic to merge unions for example. I just looked and I can use the `UnionType::from_elements` since it does build the builder: https://github.com/Glyphack/ruff/blob/04f7fc4220591b2a15c011706d056fc7e6629ebe/crates/red_knot_python_semantic/src/types.rs#L2833

---

_Marked ready for review by @Glyphack on 2024-11-20 20:31_

---

_@MichaReiser reviewed on 2024-11-20 20:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4574 on 2024-11-20 20:35_

That makes sense. I'm just surprised that creating an union from one element would result in a different return type than just that type (shouldn't `infer_type_expression` refine that type already?). Looking at the `UnionBuilder::add`, it seems that `UnionBuilder::default().add(ty).build() == ty` 

---

_@Glyphack reviewed on 2024-11-20 20:54_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4574 on 2024-11-20 20:54_

Thanks I understand your point now. I removed creating union from a single element.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/union.md`:21 on 2024-11-20 21:09_

I'm not sure why mypy and pyright do this! I can find plenty of comments in both bug trackers from maintainers clearly stating that `bool` is a subtype of `int`. And even more weirdly, mypy sometimes does simplify `bool | int`: https://mypy-play.net/?mypy=latest&python=3.12&gist=2566490340562ba079a91c53e88c1141

It seems like pyright just generally doesn't simplify subtypes out of unions, it's not specific to `bool | int`: https://pyright-play.net/?code=MYGwhgzhAECCBc0B0KBQpIwEIApYEpEUlVUATAUwDNoqd9oBaAPjmgB9osi1UAnCgDcKYEAH0ALgE8ADhRx18%2BIA

In any case, while interesting, I don't think this is a TODO for us to change, so I would remove this.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/union.md`:45 on 2024-11-20 21:12_

I don't think these two lines are testing anything about union annotations. `a2` is annotated as simply `int`, not a union, and `a` has inferred type `Literal[""]` (as seen in the error message), from the assignment on line 39, again unrelated to its declared union type.

So I don't think these two lines are relevant tests for this PR or this file.

What would be relevant is trying to assign something outside of the union to `a` or `a1`, and seeing that it issues an error:
```suggestion
# error: [invalid-assignment] "Object of type `bytes` is not assignable to `int | str`"
a = b""
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4583 on 2024-11-20 21:21_

I don't think there are any tests for this arm? It would mean adding a test with an annotation like `x: Union[int]`.

Pyright emits a diagnostic on this, mypy doesn't. I don't really feel like it should be an error -- it is a useless use of `Union`, but there is a unique and clear interpretation. There could be a lint rule against it.

I agree with @MichaReiser that I don't see why we use `UnionType::from_elements` here when it should always simplify back to a single type. I think this is completely equivalent, but more efficient:
```suggestion
                    _ => self.infer_type_expression(parameters)
```

---

_@carljm approved on 2024-11-20 21:22_

Looks good! Just a few small things.

---

_@carljm reviewed on 2024-11-20 21:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4581 on 2024-11-20 21:22_

Just re-commenting that we should have a test exercising this arm (something like `x: Union[int]`)

---

_Comment by @carljm on 2024-11-20 21:29_

> Is it possible to auto tag my PR as redknot by using a specific word in the description or title?

No, unfortunately we don't have anything like that set up. It would be nice to have!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/union.md`:20 on 2024-11-20 21:31_

```suggestion
    # Since bool is a subtype of int we simplify to int here. But we do allow assigning boolean values (see below).
```

---

_@carljm approved on 2024-11-20 21:39_

Awesome, thank you! Will commit my one remaining comment tweak, and then merge!

---

_Comment by @carljm on 2024-11-20 21:47_

Noticed a couple more small changes on reviewing the latest, pushed those too...

---

_@AlexWaygood reviewed on 2024-11-20 21:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4573 on 2024-11-20 21:50_

```suggestion
                    t.iter().map(|elt| self.infer_type_expression(elt)),
```

You can iterate directly over the tuple

---

_Merged by @carljm on 2024-11-20 21:55_

---

_Closed by @carljm on 2024-11-20 21:55_

---

_@sharkdp reviewed on 2024-11-21 10:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4576 on 2024-11-21 10:33_

This is extremely subtle, but with this change, we now fail to infer a type for the `ast::Expr::Tuple` expression itself. We only infer types for the sub-expressions via `self.infer_type_expression(elt)`, but we don't store a type for the overall `ast::Expr::Tuple` (only for the whole slice expression).

In other words, when we have something like `Union[str, int]`, we do infer types for the whole slice expression `Union[str, int]`. We also infer types for `int` and `str`, but we do not infer/store a type for the `int, str` tuple expression.

Unfortunately, the test that would have caught this is currently deactivated (see #14391), but I hope we can re-activate it soon.

I'll open a PR with a fix (Edit: https://github.com/astral-sh/ruff/pull/14510).

---

_Branch deleted on 2025-05-21 20:26_

---
