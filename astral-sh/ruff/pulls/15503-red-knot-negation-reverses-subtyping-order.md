```yaml
number: 15503
title: "[red-knot] Negation reverses subtyping order"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/another-property-test
created_at: 2025-01-15T15:21:10Z
updated_at: 2025-01-15T17:26:58Z
url: https://github.com/astral-sh/ruff/pull/15503
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Negation reverses subtyping order

---

_Pull request opened by @sharkdp on 2025-01-15 15:21_

## Summary

If `S <: T`, then `~T <: ~S`. This test currently fails with example like:

```
S = tuple[()]
T = ~Literal[True] & ~Literal[False]
```

`T` is equivalent to `~(Literal[True] | Literal[False])` and therefore equivalent to `~bool`, but the minimal example for a failure is what is stated above. We correctly recognize that `S <: T`, but fail to see that `~T <: ~S`, i.e. `bool <: ~tuple[()]`.

This is why the tests goes into the "flaky" section as well.

## Test Plan

```
export QUICKCHECK_TESTS=100000
while cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::flaky::negation_reverses_subtype_order; do :; done
```

---

_Label `red-knot` added by @sharkdp on 2025-01-15 15:21_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 15:21_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 15:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 15:21_

---

_@AlexWaygood approved on 2025-01-15 15:23_

---

_Comment by @github-actions[bot] on 2025-01-15 15:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2025-01-15 15:32_

---

_Closed by @sharkdp on 2025-01-15 15:32_

---

_Branch deleted on 2025-01-15 15:32_

---

_Comment by @carljm on 2025-01-15 17:26_

Hmm, currently we almost never can make any subtyping conclusion about a negation type (intersection with no positive elements), which is probably a lot of what keeps this test in "flaky" category.

But this test makes me realize that we probably could draw a lot more conclusions in the `Type::Intersection` branch of `is_subtype_of` by applying exactly this rule: negate both types and check subtyping in the reverse direction.

---
