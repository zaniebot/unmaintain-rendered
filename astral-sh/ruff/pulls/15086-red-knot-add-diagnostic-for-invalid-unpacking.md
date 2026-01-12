```yaml
number: 15086
title: "[red-knot] Add diagnostic for invalid unpacking"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-diagnostic
created_at: 2024-12-20T17:11:58Z
updated_at: 2024-12-30T07:40:50Z
url: https://github.com/astral-sh/ruff/pull/15086
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Add diagnostic for invalid unpacking

---

_@dhruvmanila_

## Summary

Part of #13773 

This PR adds diagnostics when there is a length mismatch during unpacking between the number of target expressions and the number of types for the unpack value expression.

There are 3 cases of diagnostics here where the first two occurs when there isn't a starred expression and the last one occurs when there's a starred expression:
1. Number of target expressions is **less** than the number of types that needs to be unpacked
2. Number of target expressions is **greater** then the number of types that needs to be unpacked
3. When there's a starred expression as one of the target expression and the number of target expressions is greater than the number of types

Examples for all each of the above cases:
```py
# red-knot: Too many values to unpack (expected 2, got 3) [lint:invalid-assignment]
a, b = (1, 2, 3)

# red-knot: Not enough values to unpack (expected 2, got 1) [lint:invalid-assignment]
a, b = (1,)

# red-knot: Not enough values to unpack (expected 3 or more, got 2) [lint:invalid-assignment]
a, *b, c, d = (1, 2)
```

The (3) case is a bit special because it uses a distinct wording "expected n or more" instead of "expected n" because of the starred expression.

### Location

The diagnostic location is the target expression that's being unpacked. For nested targets, the location will be the nested expression. For example:

```py
(a, (b, c), d) = (1, (2, 3, 4), 5)
#   ^^^^^^
#   red-knot: Too many values to unpack (expected 2, got 3) [lint:invalid-assignment]
```

For future improvements, it would be useful to show the context for why this unpacking failed. For example, for why the expected number of targets is `n`, we can highlight the relevant elements for the value expression.

In the **ecosystem**, **Pyright** uses the target expressions for location while **mypy** uses the value expression for the location. For example:

```py
if 1:
#          mypy: Too many values to unpack (2 expected, 3 provided)  [misc]
#          vvvvvvvvv
	a, b = (1, 2, 3)
#   ^^^^
#   Pyright: Expression with type "tuple[Literal[1], Literal[2], Literal[3]]" cannot be assigned to target tuple
#     Type "tuple[Literal[1], Literal[2], Literal[3]]" is incompatible with target tuple
#       Tuple size mismatch; expected 2 but received 3 [reportAssignmentType]
#   red-knot: Too many values to unpack (expected 2, got 3) [lint:invalid-assignment]
```

## Test Plan

Update existing test cases TODO with the error directives.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-20 17:11_

---

_Comment by @github-actions[bot] on 2024-12-20 17:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-12-24 09:58_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-24 09:58_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-24 09:58_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-24 09:58_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-24 09:58_

---

_@dhruvmanila reviewed on 2024-12-24 10:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:173 on 2024-12-24 10:00_

Additional changes:
* Expanded the comments in this method
* Avoid `&mut self` as it's not required (not sure why clippy didn't catch that)
* Pass the `expr` which is required for the diagnostic location

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:543 on 2024-12-24 16:19_

Re-commenttng here,
Since `"c"` doesn't have enough values to unpack, I think `LiteralString` will never put at here.

---

_@T-256 reviewed on 2024-12-24 16:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:64 on 2024-12-24 17:13_

Can we include the error code here as well (and in all cases below)?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:543 on 2024-12-24 17:17_

I agree it would be better if this were just `Unknown | Literal[3, 5]`.

---

_@carljm approved on 2024-12-24 17:20_

Very nice!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:543 on 2024-12-24 17:28_

I think to do this consistently would mean changing a bunch of other cases, though -- it would mean that e.g. for `(a, b, c) = (1, 2)` we would infer `Unknown` (or `Never` would also be reasonable) for all of `a`, `b`, `c`, rather than inferring `Literal[1]` for `a` and `Literal[2]` for `b`. That is, today we try to match up and infer as many types as we can, even if the overall unpacking is an error; this would mean changing that.

The advantage of the current approach is that it can catch more errors at once, assuming the parts that we are able to line up are actually lined up correctly. But that may not be a reasonable assumption; if you have a mis-alignment it could actually be early in the unpacking, in which case we are just inferring wrong types for many of the targets, potentially leading to false positives in subsequent code.

It looks like our current behavior is more like pyright; mypy always just infers `Any` for all targets in a mis-aligned unpacking.

I do think that the mypy behavior is more principled and we should probably switch to that, though I also don't think it's high priority to make that change.

---

_@carljm reviewed on 2024-12-24 17:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:543 on 2024-12-30 07:37_

Yes, I did that for the advantage that you've mentioned and I'm fine changing the behavior as it's an error case and should be less often in practice. I'll expand the original issue with this task.

---

_@dhruvmanila reviewed on 2024-12-30 07:37_

---

_Merged by @dhruvmanila on 2024-12-30 07:40_

---

_Closed by @dhruvmanila on 2024-12-30 07:40_

---

_Branch deleted on 2024-12-30 07:40_

---
