```yaml
number: 15297
title: "[red-knot] improve type shrinking coverage in red-knot property tests"
type: pull_request
state: merged
author: rtpg
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: type-shrinking
created_at: 2025-01-06T05:55:43Z
updated_at: 2025-01-07T09:09:18Z
url: https://github.com/astral-sh/ruff/pull/15297
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] improve type shrinking coverage in red-knot property tests

---

_@rtpg_

## Summary

While looking at #14899, I looked at seeing if I could get shrinking on the examples. It turned out to be straightforward, with a couple of caveats.

I'm calling `clone` a lot during shrinking. Since by the shrink step we're already looking at a test failure this feels fine? Unless I misunderstood `quickcheck`'s core loop

When shrinking `Intersection`s, in order to just rely on `quickcheck`'s `Vec` shrinking without thinking about it too much, the shrinking strategy is:
- try to shrink the negative side (keeping the positive side the same)
- try to shrink the positive side (keeping the negative side the same)

This means that you can't shrink from `(A & B & ~C & ~D)` directly to `(A & ~C)`! You would first need an intermediate failure at `(A & B & ~C)` or `(A & ~C & ~D)`. This feels good enough. Shrinking the negative side first also has the benefit of trying to strip down negative elements in these intersections.

## Test Plan
`cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` still fails as it current does on `main`, but now the errors seem more minimal.


---

_Review requested from @carljm by @rtpg on 2025-01-06 05:55_

---

_Review requested from @MichaReiser by @rtpg on 2025-01-06 05:55_

---

_Review requested from @AlexWaygood by @rtpg on 2025-01-06 05:55_

---

_Review requested from @sharkdp by @rtpg on 2025-01-06 05:55_

---

_Comment by @github-actions[bot] on 2025-01-06 06:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-06 07:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:165 on 2025-01-06 07:35_

Nit: Can you tell me more about what the benefits of the `chain` macro are over the `Iterator::chain` method?
```suggestion

                    // we shrink negative constraints first, as
                    // intersections with only negative constraints are
                    // more confusing
                    neg.shrink().map(move |shrunk_neg| Ty::Intersection {
                        pos: pos_orig.clone(),
                        neg: shrunk_neg,
                    }).chain(pos.shrink().map(move |shrunk_pos| Ty::Intersection {
                        pos: shrunk_pos,
                        neg: neg_orig.clone(),
                    })
```

I'd otherwise prefer the `std` chain method because it's what most people are familiar with

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:130 on 2025-01-06 07:38_


```suggestion
                1 => Some(elts.into_iter().next().unwrap()),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:135 on 2025-01-06 07:38_


```suggestion
                1 => Some(elts.into_iter().next().unwrap()),
```

---

_@MichaReiser reviewed on 2025-01-06 07:41_

---

_Label `testing` added by @MichaReiser on 2025-01-06 07:42_

---

_Label `red-knot` added by @MichaReiser on 2025-01-06 07:42_

---

_@rtpg reviewed on 2025-01-06 10:08_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/property_tests.rs`:165 on 2025-01-06 10:08_

it's easier to swap around elements of `chain!` ad-hoc, since they're on the same level of the AST.

 I also think aesthetically that it makes sense for the components of the chain to be at the same level, rather than the second one to be chained off of the first. But that's _purely_ an aesthetics thing.

I'll switch over to `a.chain(b)`.

---

_Comment by @rtpg on 2025-01-06 11:18_

I believe to have handled all the comments in the first review and have pushed up a new version

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:128 on 2025-01-06 13:08_

It might not be completely unreasonable to try and return `Ty::Never` here?

This also makes me think: I think we currently only try shrinking for the root of the `Type` tree? Like if we have `tuple[A | B, C]`, I think we would only try to remove either `A | B` from the tuple, or try to remove `C` from the tuple, i.e. we try to shrink to `C` or to `A | B`, but we would not try to shrink the `A | B` union.

---

_@sharkdp reviewed on 2025-01-06 13:10_

Thank you very much for working on this!

If it's not too much effort, it would be great to see some evidence that shrinking actually gets better. Is it possible to pin the random seed in quickcheck and then show a few before/after results?

---

_@rtpg reviewed on 2025-01-06 21:49_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/property_tests.rs`:128 on 2025-01-06 21:49_

Calling shrink on the vector of types actually does attempt to shrink the elements of the vector as well as trying to remove elements, so we get nice shrinking just leaning on the shrinking built into quickcheck (at least that was my read of the source)

---

_Comment by @rtpg on 2025-01-06 22:28_

@sharkdp do you know how to pin the quickcheck seed? I'm looking through the quickcheck source and readme and not seeing anything, but I am famously bad at finding things that are obvious.

---

_@rtpg reviewed on 2025-01-06 22:29_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/property_tests.rs`:128 on 2025-01-06 22:29_

Regarding the empty union shrinking to `Ty::Never`... feels fine by me! Especially now that I fully grok that `Ty` is a smart constructor for `Type` so I don't need to worried about types being in the "wrong location", so to speak

---

_Comment by @sharkdp on 2025-01-07 09:01_

> @sharkdp do you know how to pin the quickcheck seed?

No, I just hoped there would be a way. But it does not seem to be the case: https://github.com/BurntSushi/quickcheck/pull/278

In this case, I'm okay with your "now the errors seem more minimal" observation.

---

_@sharkdp reviewed on 2025-01-07 09:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:128 on 2025-01-07 09:05_

> Calling shrink on the vector of types actually does attempt to shrink the elements of the vector as well as trying to remove elements

:+1: 

> Regarding the empty union shrinking to `Ty::Never`... feels fine by me! Especially now that I fully grok that `Ty` is a smart constructor for `Type` so I don't need to worried about types being in the "wrong location", so to speak

If we try to shrink at all "depths", I think we can skip this. The idea was to simplify nested `Ty` unions like `A | B | (C | D)` to `A | B | Never` and then further down to `A | B`, but there are many other possible shrinking-paths that would lead to that result (e.g. via the single-element union or via a element-removal shrink on the outer union).

---

_@sharkdp approved on 2025-01-07 09:09_

Thank you for this.

---

_Merged by @sharkdp on 2025-01-07 09:09_

---

_Closed by @sharkdp on 2025-01-07 09:09_

---
