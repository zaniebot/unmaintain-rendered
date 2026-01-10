```yaml
number: 18066
title: "[ty] Support property tests for `BoundSuper`"
type: pull_request
state: open
author: cake-monotone
labels:
  - ty
assignees: []
base: main
head: cake/property-test-for-bound-super
created_at: 2025-05-13T12:29:21Z
updated_at: 2025-06-10T15:55:20Z
url: https://github.com/astral-sh/ruff/pull/18066
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Support property tests for `BoundSuper`

---

_Pull request opened by @cake-monotone on 2025-05-13 12:29_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR includes the following changes:

-  Fix incorrect `disjoint_from` behavior for `BoundSuper` types with dynamic parameters

- Add `Ty::BoundSuper` cases to property tests

## Test Plan

1. Property tests for `BoundSuper`
- `QUICKCHECK_TESTS=10000 cargo test -p ty_python_semantic -- --ignored types::property_tests::stable`

2. Regression tests for incorrect disjoint_from behavior (mdtest)
- `cargo test -p ty_python_semantic --test mdtest -- disjoint`



---

_Review requested from @carljm by @cake-monotone on 2025-05-13 12:29_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-05-13 12:29_

---

_Review requested from @sharkdp by @cake-monotone on 2025-05-13 12:29_

---

_Review requested from @dcreager by @cake-monotone on 2025-05-13 12:29_

---

_Comment by @github-actions[bot] on 2025-05-13 12:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Actual type `FullBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 650 diagnostics

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-05-13 13:07_

---

_@carljm approved on 2025-05-13 19:58_

Nice, thank you!

---

_Comment by @carljm on 2025-05-13 19:59_

Can you run with `QUICKCHECK_TESTS=1000000`? We've seen infrequent failures that show up in CI later be consistently missed with lower quickcheck counts.

---

_Comment by @cake-monotone on 2025-05-14 14:27_

> Can you run with `QUICKCHECK_TESTS=1000000`? We've seen infrequent failures that show up in CI later be consistently missed with lower quickcheck counts.

Okay!

---

_Comment by @cake-monotone on 2025-05-14 14:36_

I tried it out and got an error related to tuple.

It seems like `tuple` is never disjoint from any instance type, for example `is_disjoint_from(tuple[()], int)` returns False:
https://play.ty.dev/29a090cb-4019-4b2b-a554-8b80e86ff457

I think this is a related issue with #18004. Similar to [#18004](https://github.com/astral-sh/ruff/pull/18004), we could temporarily work around this by disabling the following code:
https://github.com/astral-sh/ruff/blob/5f3d8f6b21ebd8afa91a8fec8383defbebfb7408/crates/ty_python_semantic/src/types/property_tests/type_generation.rs#L321-L325

But the quality of the daily property tests might suffer a bit until it's properly fixed.
Shall we try disabling tuple generation for now?

---

_Comment by @carljm on 2025-05-16 14:18_

> It seems like `tuple` is never disjoint from any instance type

I think this is correct based on our current definition of the tuple type as including potential user subclasses of `tuple`, which might multiply-inherit from the other type as well. (Well, in the specific case of `int` we could consider it disjoint due to "multiple bases have instance layout conflict", but we don't model that yet.)

What specific property test does this end up failing?

---

_Comment by @cake-monotone on 2025-05-17 10:08_

> What specific property test does this end up failing?

```
all_fully_static_type_pairs_are_subtype_of_their_union
[AlwaysFalsy, tuple[()] | ~super(int, int)]
```

which means `tuple[()] | ~super(int, int)` is expected to be a subtype of  `AlwaysFalsy | tuple[()] | ~super(int, int)`.

At this moment, `AlwaysFalsy | tuple[()] | ~super(int, int)` will become `~super(int, int)` 

its because:
- AlwaysFalsy | tuple[()] = AlwaysFalsy  (`tuple[()]` is a subtype of `AlwaysFalsy`)
- AlwaysFalsy | ~super<int, int> = ~super<int, int> (`super<int, int>.bool()` == `AlwaysTrue`)

However, `tuple[()] | ~super(int, int)` is not a subtype of `~super(int, int)`

its because:
- `tuple[()]` is not subtype of `~super(int, int)`  (I guess it's wrong)

If my understanding is correct, two things need to be addressed:
-  The empty tuple literal `tuple[()]` should be handled more accurately.
-  The `BoundSuper` type should represent a set of special instances (not including custom child class of `super`). Therefore, `is_disjoint_from(tuple[...], BoundSuper)` should return `true`, unlike with typical instances.


---

_Comment by @carljm on 2025-06-10 15:54_

Sorry for delay getting back to this. Our latest thinking is that we do want empty tuple to continue to be a subtype of `AlwaysFalsy` (and we will forbid user subclasses of tuple from overriding `__bool__`, to make this sound). I think this means that our handling of empty tuple is OK here, but we do need to fix that `BoundSuper` should be a more limited type, and disjoint from arbitrary instances. It seems like that should be sufficient here?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-10 15:55_

---
