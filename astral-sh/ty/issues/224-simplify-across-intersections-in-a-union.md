```yaml
number: 224
title: simplify across intersections in a union
type: issue
state: open
author: carljm
labels:
  - set-theoretic types
assignees: []
created_at: 2024-12-16T18:55:18Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/224
synced_at: 2026-01-12T15:54:22Z
```

# simplify across intersections in a union

---

_@carljm_

When we add multiple intersection types to a union, we miss some opportunities to simplify across those intersections.

If two intersections in a union share the same `positive` elements, and they have negative elements that are disjoint from each other, the disjoint negative elements can be removed entirely.

Assume that `X` and `Y` are disjoint types:

We can transform `(A & ~X) | (A & ~Y)` to `A`.

Similarly we can transform `(A & B & ~X) | (A & B & ~Y)` to `A & B`.

And if we have `(A & ~X & ~Z) | (A & ~Y & ~Z)`, this can become `A & ~Z`.

In some cases we can eliminate them and it doesn't result in simplifying the union away. For example (assuming `T` and `U` are not disjoint from each other) `(A & ~X & ~T) | (A & ~Y & ~U)` can become `(A & ~T) | (A & ~U)`.

Here's a test:

```py
def f(x: int, y: int, flag: bool):
    if x != 1 and y != 2:
        reveal_type(x)  # revealed: int & ~Literal[1]
        reveal_type(y)  # revealed: int & ~Literal[2]
        z = x if flag else y
        reveal_type(z)  # revealed: int
```

Currently on the last line we reveal `int & ~Literal[1] | int & ~Literal[2]`, when it should just be `int`.

---

_Label `help wanted` added by @carljm on 2024-12-16 18:55_

---

_Comment by @AlexWaygood on 2024-12-16 19:14_

> Similarly we can transform `A & B & ~X | A & B & ~Y` to `A & B`.

Could you possibly add some parentheses to these examples to clarify the operator precedence? I can never remember whether `|` binds more tightly than `&`, or vice versa 😆

---

_Comment by @carljm on 2024-12-16 19:16_

> add some parentheses

Done.

---

_Comment by @AlexWaygood on 2024-12-16 19:19_

> In some cases we can eliminate them and it doesn't result in simplifying the union away, e.g. `(A & ~X & ~T) | (A & ~Y & ~U)` can become `(A & ~T) | (A & ~U)`.

Just to clarify: in this example `X`/`Y` are disjoint (so can be eliminated), but `T`/`U` are not disjoint (so cannot be eliminated)?

---

_Comment by @carljm on 2024-12-16 19:23_

> Just to clarify: in this example `X`/`Y` are disjoint (so can be eliminated), but `T`/`U` are not disjoint (so cannot be eliminated)?

Right, all of the examples are operating under the "Assume that `X` and `Y` are disjoint types:" header (with no other assumed disjointness).

---

_Comment by @AlexWaygood on 2024-12-16 19:25_

Yes, I saw that your writeup explicitly stated that `X` and `Y` should be assumed to be disjoint, but didn't see anything that explicitly stated whether `T` and `U` were supposed to also be disjoint from each other or not :-)

just wanted to clarify that my understanding was correct for why `X`/`Y` were being treated differently to `T`/`U` in that example

---

_Comment by @charliermarsh on 2024-12-29 20:17_

Can we describe this in terms of boolean logic? Could we reuse the SMT solver we have in uv for this?

---

_Comment by @carljm on 2024-12-30 00:15_

It's possible that all of our set-theoretic types could be represented as BDDs and we could use SMT to resolve subtyping, though I haven't worked through how that would look; the approaches I've seen rely on disjoint atomic types, which is an assumption we can't make because of inheritance. But this would be a whole-sale reworking of how we represent set-theoretic types, whereas the improvement suggested in this issue is more narrowly targeted.

---

_Comment by @cake-monotone on 2025-01-10 16:17_

I think this simplification is quite challenging, especially as the number of negative elements increases.

Here's a counterexample for `(A & ~X & ~T) | (A & ~Y & ~U)`:

`(int & ~Literal[False] & ~AlwaysTruthy) | (int & ~Literal[1] & ~bool)`

This satisfies the following conditions:
- `Literal[False]` is disjoint from `Literal[1]`.
- `AlwaysTruthy` is not disjoint from `bool`.

However, we cannot simplify this expression to `(int & ~AlwaysTruthy) | (int & ~bool)`.  
Notably, `False` and `1` are not elements of `(int & ~Literal[False] & ~AlwaysTruthy) | (int & ~Literal[1] & ~bool)`, but they are members of `(int & ~AlwaysTruthy) | (int & ~bool)` (because `False` is in `(int & ~AlwaysTruthy)` and `1` is in `(int & ~bool)`).


---

_Comment by @cake-monotone on 2025-01-10 17:09_

In a more generalized form, consider the following set based on DNF:

$$
(A \cap \sim T_{11} \cap \sim T_{12} \cap \dots) \cup (A \cap \sim T_{21} \cap \dots) \cup \dots \cup (A \cap \sim T_{M1} \cap \dots)
$$

Actually, all $T$ that are disjoint from $\bigcap_{i=1}^{M} \bigcup_{j=1} T_{ij}$ can be ignored.

It's because:

$$
(A \cap \sim T_{11} \cap \sim T_{12} \cap \dots) \cup (A \cap \sim T_{21} \cap \dots) \cup \dots \cup (A \cap \sim T_{M1} \cap \dots)
= A - \bigcap_{i=1}^{M} \bigcup_{j=1} T_{ij}
$$

and a disjointed $T$ cannot affect $\bigcap_{i=1}^{M} \bigcup_{j=1} T_{ij}$


NOTE: This generalization may not be practical for implementation.

---

_Comment by @carljm on 2025-01-10 17:10_

> Here's a counterexample

Good catch! Yes, the `(A & ~X & ~T) | (A & ~Y & ~U)` case is wrong as stated. I think that for the simplification to be valid, there is an additional requirement that `X` and `U` be disjoint, and `Y` and `T` be disjoint.

I think a simpler way to put it is that the simplification is restricted to `(A & ~X) | (A & ~Y) -> A` when `X` and `Y` are disjoint, but we can rewrite `(A & ~X & ~T) | (A & ~Y & ~U)` as `(A & ~(X | T)) | (A & ~(Y | U))`, in which case it fits the first form, as long as `(X | T)` is disjoint from `(Y | U)`. It may not be practical / worth it to check this transformation when there are multiple negative elements.

Given that astral-sh/ruff#15400 already fixed the one actual observed case, I think this issue in general is low priority unless we see other cases crop up in realistic code.

---

_Comment by @cake-monotone on 2025-01-11 06:40_

Got it! Also, a hasty implementation might significantly impact the performance of the `UnionBuilder`. I agree that it would be better to lower its priority and implement it later.

---

_Label `help wanted` removed by @carljm on 2025-03-15 16:23_

---

_Renamed from "[red-knot] simplify across intersections in a union" to "simplify across intersections in a union" by @MichaReiser on 2025-05-07 15:27_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:19_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:00_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
