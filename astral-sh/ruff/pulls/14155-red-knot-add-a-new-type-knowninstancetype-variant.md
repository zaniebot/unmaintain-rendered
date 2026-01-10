```yaml
number: 14155
title: "[red-knot] Add a new `Type::KnownInstanceType` variant"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/known-instance-type
created_at: 2024-11-07T14:42:16Z
updated_at: 2024-11-08T23:24:06Z
url: https://github.com/astral-sh/ruff/pull/14155
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Add a new `Type::KnownInstanceType` variant

---

_Pull request opened by @AlexWaygood on 2024-11-07 14:42_

## Summary

Fixes #14114. I don't think I can really describe the problems with our current architecture (and therefore the motivations for this PR) any better than @carljm did in that issue, so I'll just copy it out here!

---

We currently represent "known instances" (e.g. special forms like `typing.Literal`, which are an instance of `typing._SpecialForm`, but need to be handled differently from other instances of `typing._SpecialForm`) as an `InstanceType` with a `known` field that is `Some(...)`.

This makes it easy to handle a known instance as if it were a regular instance type (by ignoring the `known` field), and in some cases (e.g. `Type::member`) that is correct and convenient. But in other cases (e.g. `Type::is_equivalent_to`) it is not correct, and we currently have a bug that we would consider the known-instance type of `typing.Literal` as equivalent to the general instance type for `typing._SpecialForm`, and we would fail to consider it a singleton type or a single-valued type (even though it is both.)

An instance type with `known.is_some()` is semantically quite different from an instance type with `known.is_none()`. The former is a singleton type that represents exactly one runtime object; the latter is an open type that represents many runtime objects, including instances of unknown subclasses. It is too error-prone to represent these very-different types as a single `Type` variant. We should instead introduce a dedicated `Type::KnownInstance` variant and force ourselves to handle these explicitly in all `Type` variant matches.

## Possible followups

There is still a little bit of awkwardness in our current design in some places, in that we first infer the symbol `typing.Literal` as a `_SpecialForm` instance, and then later convert that instance-type into a known-instance-type. We could also use this `KnownInstanceType` enum to account for other special runtime symbols such as `builtins.Ellipsis` or `builtins.NotImplemented`.

I think these might be worth pursuing, but I didn't do them here as they didn't seem essential right now, and I wanted to keep the diff relatively minimal.

## Test Plan

`cargo test -p red_knot_python_semantic`. New unit tests added for `Type::is_subtype_of`.


---

_Label `red-knot` added by @AlexWaygood on 2024-11-07 14:42_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-07 14:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-07 14:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-07 14:42_

---

_Renamed from "[red-knot] Add a new Type::KnownInstanceType variant" to "[red-knot] Add a new `Type::KnownInstanceType` variant" by @AlexWaygood on 2024-11-07 14:42_

---

_Comment by @github-actions[bot] on 2024-11-07 14:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to pull: error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 6437 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:338 on 2024-11-07 19:27_

nit: I don't think these objects are particularly special "at runtime" -- it's in the type system where they are special! And I think it's useful if these doc comments always talk about the set of objects represented: "a specific symbol" doesn't do that clearly.

```suggestion
    /// A single Python object that requires special treatment in the type system
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:358 on 2024-11-07 19:41_

We do this a lot in this PR, I wonder about a `Class::to_instance_type()` method for it?

---

_@carljm approved on 2024-11-07 19:42_

Looks great, thank you!!

---

_Comment by @carljm on 2024-11-07 21:57_

I think I might resolve conflicts and land this, if that's OK with you? I'm realizing that I think I want to represent a TypeVar (not as a type, but the type of the typevar's own symbol, e.g. `T` in `def f[T](): ...`) as a `KnownInstanceType` with fallback type `typing.TypeVar`.

---

_@carljm reviewed on 2024-11-07 21:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:358 on 2024-11-07 21:59_

This can be considered separately as a follow-up if it's worth it

---

_Comment by @AlexWaygood on 2024-11-07 22:00_

Go for it! It's late here and I'm still travelling back from choir 👍

---

_Merged by @carljm on 2024-11-07 22:07_

---

_Closed by @carljm on 2024-11-07 22:07_

---

_Branch deleted on 2024-11-07 22:07_

---

_@AlexWaygood reviewed on 2024-11-08 23:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:358 on 2024-11-08 23:24_

https://github.com/astral-sh/ruff/pull/14215

---
