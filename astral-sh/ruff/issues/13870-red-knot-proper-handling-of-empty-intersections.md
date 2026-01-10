```yaml
number: 13870
title: "[red-knot] Proper handling of empty intersections and Never."
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - great writeup
  - ty
assignees: []
created_at: 2024-10-21T21:41:30Z
updated_at: 2024-10-22T19:02:47Z
url: https://github.com/astral-sh/ruff/issues/13870
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Proper handling of empty intersections and Never.

---

_Issue opened by @sharkdp on 2024-10-21 21:41_

I found this while creating the type `Literal[1, 2, 3] & ~Literal[1] & ~Literal[2]`. I was expecting to get `Literal[3]` after all simplifications were applied, but I got `~Literal[2] | Literal[3]` instead. This is clearly not optimal.

The reason for this behavior is the following (using `1` instead of `Literal[1]` etc): When adding the first negation, we get the following in the union-of-intersections representation:
```
  (1 | 2 | 3) & ~1
= (1 & ~1 | 2 & ~1 | 3 & ~1)
```
In the second and third component, the simplification creates `2 & ~1 = 2` and `3 & ~1 = 3`, as expected. In the first component, the current implementation of simplification removes all positive (1) and negative (~1) components from the intersection, leaving an empty intersection:
```
= (empty-intersection | 2 | 3)
```
The idea was that the `empty-intersection` would represent `Never` (similar to how `IntersectionBuilder::build()` returns `Never` for the empty-empty case). But an empty intersection should actually represent the type `object`. To represent `Never`, we would need to explicitly add `Never` to the positive side. Since we don't do that, we end up creating
```
  (empty-intersection | 2 | 3) & ~2
= (~2 | 2 & ~2 | 3 & ~2)
= ~2 | Never | 3
= ~2 | 3
```
in the second step.

---

_Label `bug` added by @sharkdp on 2024-10-21 21:41_

---

_Label `red-knot` added by @sharkdp on 2024-10-21 21:41_

---

_Assigned to @sharkdp by @sharkdp on 2024-10-21 21:44_

---

_Label `great writeup` added by @AlexWaygood on 2024-10-21 21:50_

---

_Closed by @sharkdp on 2024-10-22 11:32_

---

_Comment by @sharkdp on 2024-10-22 11:34_

Hey GitHub, I said *"while **trying** to fix #13876"* in that PR.

---

_Reopened by @sharkdp on 2024-10-22 11:34_

---

_Comment by @sharkdp on 2024-10-22 13:35_

~~The fundamental problem is the following. In `bindings_ty`, we use `IntersectionBuilder::add_positive` with constraints, as if that would mean: build the intersection with the existing `binding_ty` type:~~

https://github.com/astral-sh/ruff/blob/7dbd8f0f8e725d05cc1d15e6ef1c6cc2ed2b2090/crates/red_knot_python_semantic/src/types.rs#L156-L161

~~But the intersection between `A & B` is subtly different from `IB::new().add_positive(A).add_positive(B).build()`, if `A` and/or `B` are intersections themselves. For example, if `A = {pos = [p]; neg = []}` and `B = {pos = []; neg = [n]}`, using `.add_positive(A).add_positive(B)` will result in an intersection with `{pos = [p]; neg = [n]}`. However, `B` is actually `Never`, so the intersection between `A & B` is also `Never`.~~

~~Note: Those intermediate empty positive/negative lists can appear while building up a larger intersection type (due to simplification), which is exactly what happens in the code above.~~

~~I will try to fix this by adding a `Type::intersect` method.~~

---

_Renamed from "[red-knot] Intersections with only negative contributions should be equal to Never" to "[red-knot] Proper handling of empty intersections and Never." by @sharkdp on 2024-10-22 18:45_

---

_Closed by @sharkdp on 2024-10-22 19:02_

---

_Closed by @sharkdp on 2024-10-22 19:02_

---
