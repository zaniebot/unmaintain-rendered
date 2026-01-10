```yaml
number: 13257
title: "[red-knot] Add a new type representing the set of objects available on a certain Python version"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: sys-version-info
created_at: 2024-09-05T18:15:50Z
updated_at: 2024-10-26T18:28:36Z
url: https://github.com/astral-sh/ruff/pull/13257
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Add a new type representing the set of objects available on a certain Python version

---

_Pull request opened by @AlexWaygood on 2024-09-05 18:15_

## Summary

Python types can and should be thought of in terms of set theory. The `int` type represents the set of all objects for which the `__class__` attribute of that object is a subclass of `int`. The `int | str` type represents the union of two sets: the first being the set of all objects for which the `__class__` attribute of that object is a subclass of `int`, the second being the set of all objects for which the `__class__` attribute of that object is a subclass of `str`.

We intend to implement support for `sys.version_info` branches using types; this will allow us to implement support for multiversion checking in the future. Some examples of how we intend this to work are:

```py
# The code is being checked with `--target-version=3.13,3.12`,
# indicating that we want it to be checked for correctness on Python 3.13 and Python 3.12 simultaneously

import sys

x = 42

if sys.version_info >= (3, 13):
    x = 42
    reveal_type(x) # Literal[42] & ExistsOnVersion[3.13]

reveal_type(x) # Literal[42] | (Literal[42] & ExistsOnVersion[3.13]) (simplifies to Literal[42])

if sys.version_info >= (3, 13):
    y = 42
    reveal_type(y) # Literal[42] & ExistsOnVersion[3.13]
else:
    y = 56
    reveal_type(y) # Literal[56] & ExistsOnVersion[3.12, 3.11, 3.10, 3.9, 3.8]

reveal_type(y) # (Literal[42] & ExistsOnVersion[3.13) | (Literal[56] & ExistsOnVersion[3.12])
```

This PR adds the `ExistsOnVersion` type and implements display for it. It is not yet inferred anywhere.

## Test Plan

`cargo test -p red_knot_python_semantic`

Co-authored-by: Carl Meyer <carl@astral.sh>

---

_Label `red-knot` added by @AlexWaygood on 2024-09-05 18:15_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-05 18:15_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-05 18:15_

---

_Comment by @github-actions[bot] on 2024-09-05 18:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-09-05 19:40_

Looks good to me, but that's not too surprising since we paired to write it 😆 feel free to wait for an independent reviewer if you want.

I did realize re-reading the design bits we wrote up that the concept of a constraint applied to a definition, at the time of definition, based on a prior check, will be new for our use-def map and require some additional work there to support. (Currently constraints apply to definitions if they appear between the definition and the use, not before the definition.) But it shouldn't be too hard to add this support.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:164 on 2024-09-06 06:13_

What's the motivation for using an enum with a fixed set of versions rather than a more generic representation of `major.minor`? Do we need to support narrowing by patch version?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:232 on 2024-09-06 06:15_

I'm inclined to instead implement the type as a specialized intersection type. Meaning, the type signature is `{ inner: Type, version: Version }`. It could help with performance because the `IntersectionType` is somewhat expensive because it needs to allocate a `HashSet`. A specialized type could avoid that allocation.

---

_@MichaReiser reviewed on 2024-09-06 06:23_

I love that you jumped right onto this and I kind of feel bad to now be the person pushing back, considering that I motivated this to large degrees. 

I think it would be good to spend some time writing a design document for this demonstrating how this new typing feature would be used, considering that we walk new terain. I'm also interested to better understand how this feature works with our current `target_version` which is always a single version. Would that need to become a version range? What changes are required to the module resolver? How do we get this version (requires-python is always a minimal version constraint. Is there a way to configure that a user wants exactly one version?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:164 on 2024-09-06 22:26_

We need a fixed set of supported versions because otherwise the implementation becomes _much_ more complex; we can no longer just use intersections and unions, we have to implement a whole dedicated system for resolving sets of inequality constraints, like uv has, just for this one case. With a fixed set of supported versions, if we see `if sys.version_info >= (3, 11)` we can easily resolve that to `ExistsOnVersion(3.11) | ExistsOnVersion(3.12) | ExistsOnVersion(3.13)`. Without it, we have to represent `sys.version_info >= (3, 11)` as an inequality, and implement constraint solving for arbitrary sets of inequality constraints.

It's _possible_, but it's a lot of extra work to take on unless we really need to. And I don't see why we'd need to; there will always be a finite and known set of Python versions supported by red-knot.

I don't think we need to support narrowing by patch version. Patch versions are not supposed to change features in ways that would require differing typing. Typeshed never narrows on patch versions, and neither pyright nor mypy support it. (Mypy doesn't even understand `sys.version_info` checks if they check the patch version; pyright still understands them, but treats them as if the patch version wasn't there at all.)

---

_@carljm reviewed on 2024-09-06 22:26_

---

_@carljm reviewed on 2024-09-06 22:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:232 on 2024-09-06 22:32_

Hmm, I kind of like this idea. I'm not totally convinced by the performance motivation: we will have _lots_ of intersections due to narrowing constraints in general, and we will need to make our intersection implementation perform as well as we possibly can; I suspect that version/platform narrowing will end up as a small overall percentage of our intersection types.

But I think it might simplify the implementation to do this, because these types _are_ special: they shouldn't ever exist outside of an intersection, and whenever we encounter them (and haven't already narrowed them away), we should treat them as if they were just the inner type. I think this will be a bit simpler than treating them as `object` and relying on intersection simplification to simplify `foo & object` to `foo`.

Note that we will have `Foo & (PythonVersion[3.11] | PythonVersion[3.12])`, which can't directly be represented this way, but it can distribute to `(Foo & PythonVersion[3.11]) | (Foo & PythonVersion[3.12])` (which we would do anyway, since we normalize to disjunctive normal form), so that should work fine with this representation.

---

_Comment by @carljm on 2024-09-06 22:42_

> I love that you jumped right onto this

Alex and I have a weekly pair-programming session on Thursdays, where we tend to jump into a random new fun thing that sounds interesting to both of us 😆 That's how we ended up getting unplanned builtins support, too.

> I'm also interested to better understand how this feature works with our current `target_version` which is always a single version. Would that need to become a version range?

This will work fine with a single target Python version, or with an arbitrary set of target Python versions. All it means is that any non-target Python version immediately resolves away to `Never`, but it doesn't matter if the non-target-version set is "all but one version" or "all but X versions". So implementing support for `sys.version` checks in this way doesn't require us to immediately start doing multi-version checking, it just leaves the door open that it will be possible in the future.

> How do we get this version (requires-python is always a minimal version constraint. Is there a way to configure that a user wants exactly one version?)

If we don't do multi-version-checking, then we kind of need to give the user a red-knot-specific config to specify exactly one version to check. (Though we could default to checking the minimum `requires-python` version.)

If we do multi-version-checking, then it's kind of cool that our default can be `requires-python` and newer, but I think we still need more granular red-knot-specific config. (What if red-knot supports the latest and greatest Python but my library hasn't added support for it yet; I should still be able to upgrade red-knot and keep using it.)

> I think it would be good to spend some time writing a design document for this

I'm not opposed to this, but I also don't think we have to answer all questions about multi-version checking as a blocker to implementing support for `sys.version_info` checks in a way that makes it possible to do multi-version checking in the future.

---

_Comment by @AlexWaygood on 2024-10-26 18:28_

After further discussion internally, we decided that our initial implementation of a type checker will _not_ support multiversion checking.

This doesn't mean that it's a feature we don't want! It's still something we'd love to do, if at all feasible. _However_, it would also come with a lot of complexities. After re-evaluating, we think other things have priority for now, and we think it should be possible to add support for multi-version checking at a later stage rather than doing it now.

TL;DR: closing this PR for now!

---

_Closed by @AlexWaygood on 2024-10-26 18:28_

---

_Branch deleted on 2024-10-26 18:28_

---
