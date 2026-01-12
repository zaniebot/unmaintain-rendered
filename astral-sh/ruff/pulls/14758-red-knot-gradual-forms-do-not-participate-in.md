```yaml
number: 14758
title: "[red-knot] Gradual forms do not participate in equivalence/subtyping"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14524
created_at: 2024-12-03T14:16:09Z
updated_at: 2024-12-04T16:11:28Z
url: https://github.com/astral-sh/ruff/pull/14758
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Gradual forms do not participate in equivalence/subtyping

---

_@sharkdp_

## Summary

This changeset contains various improvements concerning non-fully-static types and their relationships:

- Make sure that non-fully-static types do not participate in equivalence or subtyping.
- Clarify what `Type::is_equivalent_to` actually implements.
- Introduce `Type::is_fully_static`
- New tests making sure that multiple `Any`/`Unknown`s inside unions and intersections are collapsed.

closes #14524

## Test Plan

- Added new unit tests for union and intersection builder
- Added new unit tests for `Type::is_equivalent_to`
- Added new unit tests for `Type::is_subtype_of`
- Added new property test making sure that non-fully-static types do not participate in subtyping
 

---

_Label `red-knot` added by @sharkdp on 2024-12-03 14:16_

---

_Review requested from @carljm by @sharkdp on 2024-12-03 14:16_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-03 14:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-03 14:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:311 on 2024-12-03 14:17_

Checking for Type::Todo is not necessary anymore now with the changed `is_equivalent_to`. I added a new condition involving `Type::Unknown` to reproduce the previous behavior, but I'm unsure if that's really what is required here. There are no tests that fail if I remove it. And I struggled to write one.

The thinking is this:

Before, we had `Unknown.is_equivalent_to(Unknown)`. So `!first.is_equivalent_to(other)` would trigger a diagnostic if either `first` or `other` are `Unknown`, but not both.

This can be formulated as `(first == Unknown) xor (other == Unknown)`, which is the same as `(first == Unknown) != (other == Unknown)`.

---

_@sharkdp reviewed on 2024-12-03 14:17_

---

_@sharkdp reviewed on 2024-12-03 14:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 14:20_

This might be too expensive. If we don't check for fully-static here, we need to add full-static tests in various places below. Like in this branch, for example:
```rs
            (ty, Type::Union(union)) => union
                .elements(db)
                .iter()
                .any(|&elem_ty| ty.is_subtype_of(db, elem_ty)),
```
This is only true if all `elem_ty`s are fully-static. Otherwise we would conclude that `int` is a subtype of `Any | int`.

All of these cases are caught by the new property test, so the risk of forgetting something is probably low. It's still much cleaner to just add the test here. Let's see what the benchmarks say.

---

_Comment by @github-actions[bot] on 2024-12-03 14:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1029 on 2024-12-03 14:25_

`Type::KnownInstance(…)` may represent things like the type of `typing.Any`, but this is a fully-static type (with a single inhabitant), and not the same as the actual gradual type `Any`.

---

_@sharkdp reviewed on 2024-12-03 14:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:311 on 2024-12-03 21:39_

Here is one form of the test that fails without this check:

```py
# error: [unresolved-import]
# error: [unresolved-import]
from unknown import A, B

def flag() -> bool: ...

if flag():
    x: A
else:
    x: B
x = 1
```

It requires suppressing some unresolved imports in order to get something typed as `Unknown` that can be used in a declaration. And it's not entirely clear to me in this example that we _shouldn't_ error on these as incompatible declarations.

I think if we have this condition, we should have it for `Any` as well, they should be treated the same. So after https://github.com/astral-sh/ruff/pull/14742 lands, we could write the same test using `Any` annotations, without having to suppress any errors.

All that said, I am really not settled on the right form of this conflicting-declarations check. It may be that we should not have this check at all, and should just silently union the live declarations. It may be that we should use "gradual equivalence" for this check, where `Any` or `Unknown` would be compatible with any other declaration. I feel like I need to see how this works on more real codebases, and that will be difficult, since today's typed codebases don't generally have multiple declarations of the same symbol, since mypy and pyright don't allow it.

So I would suggest that we do the simplest thing for now and just remove this extra condition (and the comment), and allow the check to be as strict as possible, then relax it as we are pushed to do so by actual user needs in the future.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 21:50_

Benchmarks CI job passed with a green check, but there is no comment from codspeed? Is that normal?

If benchmarks are OK I am fine with doing this for now and considering optimizations if it shows up as a performance issue.

Another thought I have is that if we defined a cheaper `is_fully_static_shallow` which didn't descend into unions, intersections, tuples, etc, and only checked that here, would that still give the right behavior? Because if a complex type has a non-fully-static member, wouldn't we eventually hit that member in the actual subtype check and still return `false`? It may be more complex than this for intersections... also it's not entirely self-evident that this will be faster; if a high proportion of types are not fully static, it may be cheaper to rule them out quickly than to go into the actual subtype check.

---

_@carljm approved on 2024-12-03 22:00_

This is fantastic. Super clear and correct. Thank you.

---

_@MichaReiser reviewed on 2024-12-03 22:03_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 22:03_

> Benchmarks CI job passed with a green check, but there is no comment from codspeed? Is that normal?

Yes, this is normal. Codspeed only comments if there's at least a 4% regression (or if there was one in a previous push). 

There's a 1% perf regression in the cold benchmark https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-14524?uri=crates%2Fruff_benchmark%2Fbenches%2Fred_knot.rs%3A%3Acheck_file%3A%3Abenchmark_cold%3A%3Ared_knot_check_file%255Bcold%255D

---

_@carljm reviewed on 2024-12-03 22:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 22:14_

Thank you! How did you get that link to the actual results, without the comment? I looked in the detailed logs for the CI benchmark job and didn't see any link there. Do you just go to https:://codspeed.io/astral-sh/ruff and find the PR there?

---

_@carljm reviewed on 2024-12-03 22:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1030 on 2024-12-03 22:16_

I think for these three, we also need to check the MRO for an Any/Unknown base? A class that inherits Any/Unknown is not a fully static type.

---

_@sharkdp reviewed on 2024-12-03 22:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 22:21_

> Another thought I have is that if we defined a cheaper `is_fully_static_shallow` which didn't descend into unions, intersections, tuples, etc, and only checked that here, would that still give the right behavior?

Hm. It's late here and I should probably sleep first, but I think that won't work (without additional checks below). In the example above, the `int | Any` union would pass the shallow check (there is no gradual form at the top level). And then we would check if `int` is a subtype of `int | Any` using
```rs
            (ty, Type::Union(union)) => union
                .elements(db)
                .iter()
                .any(|&elem_ty| ty.is_subtype_of(db, elem_ty)),
```
which would return true because `int <: int`. We wouldn't even look at `Any` in the traversal due to short-circuiting. We *could* add another `.iter().all(|elem_ty| elem_ty.is_fully_static_shallow())` test here and that would probably be sound, but that would sort of defeat the purpose? There would be a benefit for very deep nested types, but I'm not sure it's worth the additional complexity?

---

_@sharkdp reviewed on 2024-12-03 22:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 22:24_

> Do you just go to https:://codspeed.io/astral-sh/ruff and find the PR there?

This is what I do. Go to [branches](https://codspeed.io/astral-sh/ruff/branches) and then select it there.

---

_@carljm reviewed on 2024-12-03 22:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-03 22:25_

Yes, you're right. I think if we find that we need to improve this, the thing to try might be to see if we can steal a bit from `UnionType`, `IntersectionType` etc, and track fully-static-ness eagerly as we construct the type, so that the read check can be shallow and cheap.

---

_@MichaReiser reviewed on 2024-12-04 07:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:606 on 2024-12-04 07:27_

> Thank you! How did you get that link to the actual results, without the comment?


You can click on details next to the codspeed check. Then there's another link to the benchmark results. 

![image](https://github.com/user-attachments/assets/6929631e-48c2-4e94-ab5d-e2c71f4dbfc1)




---

_@sharkdp reviewed on 2024-12-04 14:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1030 on 2024-12-04 14:02_

Yes, thanks!

For `Type::Instance(_)`, this is obviously correct.

For `Type::ClassLiteral(_)` it took me some time to understand whether we want that or not. I had a good discussion with @AlexWaygood and I believe I now understand why we want that. This also seems to match mypy's and pyright's interpretation:
```py
from typing import Any

class FullyStatic: ...

#instance1: int = FullyStatic()  # obviously doesn't work
#class1: type[int] = FullyStatic  # same

class Gradual(Any): ...

instance2: int = Gradual()  # okay, `Gradual` is a gradual type
class2: type[int] = Gradual  # also okay
```


I then went on to implement this, which seemed fairly trivial at first. But it turns out that this causes all sorts of problems. One of these problems is that we don't fully understand even the most basic types like `str`. In typeshed, `str` inherits from `Sequence[str]`, which is a complex generic protocol. This currently manifests in the following MRO for `str`, which causes us to believe it would be a non-fully-static type.
```py
tuple[Literal[str], Unknown, Literal[object]]
```
This can be patched by matching on a set of known classes. But that is far from ideal. If someone unknowingly pulls in another type from the standard library (outside the set of known classes; see #14769 as an example) with a similar problem, they would see all sorts of weird behavior without a clear indication of what's wrong.

And then there is also another problem where merely iterating over the MRO of a class here (without drawing any conclusions from it) causes us to eagerly infer some types as `Unknown`.

If someone has a good idea on how to resolve this (right now, without waiting for generics), please let me know. Otherwise, I would suggest we move forward with the TODO comment and revisit this when we understand generics.

For reference: here is what I tried: ec64bd840393aa908445720df9236b9cc11f0fb2

---

_@carljm reviewed on 2024-12-04 15:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1030 on 2024-12-04 15:41_

Fully agree with this analysis and I think the conclusion is right; we need to defer this handling until we can understand most/all typeshed classes that aren't supposed to inherit Any/Unknown, as not inheriting Any/Unknown. TODO comment (and maybe an issue on the backlog, too) seems the right move.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1044 on 2024-12-04 15:48_

This is a bit scary :/ It suggests we frequently need to ask about the fully-static-ness of a type too soon. I guess we'll cross this bridge when we come to it; hopefully we can find a way to defer the need for checking this so early.

---

_@carljm approved on 2024-12-04 15:48_

---

_Merged by @sharkdp on 2024-12-04 16:11_

---

_Closed by @sharkdp on 2024-12-04 16:11_

---

_Branch deleted on 2024-12-04 16:11_

---
