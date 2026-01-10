```yaml
number: 11992
title: "[red-knot] Reduce allocations when normalizing `VendoredPath`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: reduce-normalization-allocation
created_at: 2024-06-23T22:14:32Z
updated_at: 2024-06-24T12:08:02Z
url: https://github.com/astral-sh/ruff/pull/11992
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Reduce allocations when normalizing `VendoredPath`s

---

_Pull request opened by @AlexWaygood on 2024-06-23 22:14_

## Summary

This PR reduces allocations when normalizing `VendoredPath`s by only allocating when the path requires normalization of some kind. In most cases, the path should already be normalized, so allocating shouldn't be necessary; this PR optimized for the happy path.

## Test Plan

`cargo test -p ruff_db`. There's already quite a few tests in this file, so I think they should be sufficient.


---

_Label `red-knot` added by @AlexWaygood on 2024-06-23 22:14_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-23 22:14_

---

_Comment by @github-actions[bot] on 2024-06-23 22:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-06-24 06:31_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:322 on 2024-06-24 06:31_

I think we can simplify the implementation and also reduce the branching by doing a simple search first:

1. Test if the path contains any `\`, if so, jump right into the normalization logic
2. Test if all components are `Normal`. If so, return `Cow::Borrowed(path.as_str())`
3. Otherwise run the normlaization

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-24 10:56_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:277 on 2024-06-24 11:20_

I think I would just call into the regular normalization logic in that case. It seems unnecessary to first check if all components are normal. 

---

_@MichaReiser approved on 2024-06-24 11:20_

---

_@MichaReiser reviewed on 2024-06-24 11:20_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:23 on 2024-06-24 11:20_

argh nooo :( 

I don't like itertools lol



---

_@AlexWaygood reviewed on 2024-06-24 11:37_

---

_Review comment by @AlexWaygood on `crates/ruff_db/Cargo.toml`:23 on 2024-06-24 11:37_

Not all itertools are equal, though, right? ;) I checked out their implementation for `Iterator::join` and it looks pretty reasonable to me. But I might get rid of this again as a result of addressing your other comment.

---

_@MichaReiser reviewed on 2024-06-24 11:52_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:23 on 2024-06-24 11:52_

Yeah, it just feels like a dependency that we could possibly remove by vendoring the few methods that we need (maybe join?)

---

_@MichaReiser reviewed on 2024-06-24 11:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:23 on 2024-06-24 11:53_

I, in general, want to remove the number of Ruff's dependencies. That's why I'm a bit annoying about introducing new dependencies ;)

---

_@AlexWaygood reviewed on 2024-06-24 11:55_

---

_Review comment by @AlexWaygood on `crates/ruff_db/Cargo.toml`:23 on 2024-06-24 11:55_

Yeah I totally get that. I think removing the `itertools` dependency from Ruff altogether might be a somewhat large task as we already use it quite a lot in the linter crate. But I'm totally on board with thinking it through carefully when we add new dependencies.

I've got rid of it from this PR, anyway :-)

---

_Merged by @AlexWaygood on 2024-06-24 12:08_

---

_Closed by @AlexWaygood on 2024-06-24 12:08_

---

_Branch deleted on 2024-06-24 12:08_

---
