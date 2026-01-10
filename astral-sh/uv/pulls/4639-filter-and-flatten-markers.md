```yaml
number: 4639
title: Filter and flatten markers
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: more-marker-simplification
created_at: 2024-06-28T22:10:22Z
updated_at: 2024-07-08T18:59:22Z
url: https://github.com/astral-sh/uv/pull/4639
synced_at: 2026-01-10T13:42:52Z
```

# Filter and flatten markers

---

_Pull request opened by @ibraheemdev on 2024-06-28 22:10_

## Summary

More marker simplification:
- Filters out redundant subtrees based on outer expressions, e.g. `a and (a or b)` simplifies to `a`.
- Flattens nested trees internally, e.g. `(a and b) and c`

Resolves https://github.com/astral-sh/uv/issues/4536.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-28 22:10_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-06-28 22:10_

---

_@charliermarsh reviewed on 2024-06-30 23:44_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:97 on 2024-06-30 23:44_

Are there redundant expressions that we'd only notice after normalizing (e.g., flattening)? In other words, is `filter_all(normalize_all(...))` idempotent? Or is there some risk that we don't normalize everything by way of this ordering?

---

_@ibraheemdev reviewed on 2024-06-30 23:50_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:97 on 2024-06-30 23:50_

`filter_all` was written to handle non-flattened trees and can also leave the tree in a denormalized (not flattened) state.

Thinking about it more though, I suppose it is possible that there are duplicates that only show up after combining version ranges, so maybe we have to normalize before *and* after? I was trying to avoid this by writing `filter_all` to handle non-flattened ranges, but maybe normalizing before is unavoidable. I can try to create a test case where this happens.

---

_Comment by @charliermarsh on 2024-07-01 00:02_

Relatedly, after this change, if we have a marker that contains an `OR` with `MarkerTree::And(vec![])`, do we map that to `None` (or even `MarkerTree::And(vec![])`)? It's relevant for a separate PR I'm working on.

---

_@konstin reviewed on 2024-07-01 07:41_

---

_Review comment by @konstin on `crates/uv-resolver/src/marker.rs`:291 on 2024-07-01 07:41_

Scroll-by Nit: `//` -> `///` so it's rustdoc

---

_Comment by @ibraheemdev on 2024-07-01 16:50_

@charliermarsh it does not, does that ever come up?

---

_Comment by @charliermarsh on 2024-07-06 19:31_

@ibraheemdev -- I was considering making some changes elsewhere that would benefit from it, but it's not critical. Although I do think `OR MarkerTree::And(vec![])` should be simplified to `MarkerTree::And(vec![])`.

What's left to do here?

---

_Comment by @charliermarsh on 2024-07-07 20:42_

Another one we could do: #4852

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-08 17:00_

---

_@ibraheemdev reviewed on 2024-07-08 17:01_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:97 on 2024-07-08 17:01_

Fixed and added a test.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/marker.rs`:291 on 2024-07-08 17:52_

I might add a little more rustdoc for cases like these. As it is, I think the comment is effectively strictly redundant with the name and type signature. I think I'd write something like this:

```rust
/// For a given slice of marker trees, this collects
/// all direct leaf expressions from each marker tree,
/// and skips any marker trees that are themselves
/// not leaves.
```

I might also add some context as to _why_ this routine is useful.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/marker.rs`:324 on 2024-07-08 17:55_

Is there a reason why this doesn't also filter them out for a direct leaf expression? I guess it would need to rewrite the marker expression as `MarkerTree::And(vec![])` in that case...

Another perspective that I find helpful for routines like this is phrasing like, "returns a marker tree in which the given expression is assumed to be true in all conjunctions." I think that captures better the essence of the contract here if I'm understanding correctly.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/marker.rs`:1 on 2024-07-08 17:56_

Oh Clippy. (I agree with your stylistic choice here.)

---

_@BurntSushi approved on 2024-07-08 17:57_

I think this overall LGTM. This does seem like playing whack-a-mole, but I think we figured out it would require significant investment to do this fully correctly right?

---

_@ibraheemdev reviewed on 2024-07-08 18:41_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:291 on 2024-07-08 18:41_

Added some more documentation.

---

_@ibraheemdev reviewed on 2024-07-08 18:45_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:324 on 2024-07-08 18:45_

Yeah, that's covered by the `dedup` calls in `normalize_all`. It's a bit trickier to do that here because we want to filter recursively, but only deduplicate from the outer conjunction or disjunction, depending on the context (we don't just want to remove the expression everywhere).

---

_Merged by @ibraheemdev on 2024-07-08 18:59_

---

_Closed by @ibraheemdev on 2024-07-08 18:59_

---

_Branch deleted on 2024-07-08 18:59_

---
