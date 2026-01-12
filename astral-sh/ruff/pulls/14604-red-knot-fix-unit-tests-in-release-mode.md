```yaml
number: 14604
title: "[red-knot] Fix unit tests in release mode"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14594
created_at: 2024-11-26T09:34:43Z
updated_at: 2024-11-26T14:40:05Z
url: https://github.com/astral-sh/ruff/pull/14604
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Fix unit tests in release mode

---

_@sharkdp_

## Summary

This is about the easiest patch that I can think of. It has a drawback in that there is no real guarantee this won't happen again. I think this might be acceptable, given that all of this is a temporary thing. But there are other approaches we could take:

- We could get rid of the debug/release distinction and just add `@Todo` type metadata everywhere. This has possible affects on runtime. The main reason I didn't follow through with this is that the size of `Type` increases. We would either have to adapt the `assert_eq_size!` test or get rid of it. Even if we add messages everywhere and get rid of the file-and-line-variant in the enum, it's not enough to get back to the current release-mode size of `Type`.
- We could generally discard `@Todo` meta information when using it in tests. I think this would be a huge drawback. I like that we can have the actual messages in the mdtest. And make sure we get the expected `@Todo` type, not just any `@Todo`. It's also helpful when debugging tests.

We could also think about running tests in release mode via CI, if this is something that we need to support.

closes #14594

## Test Plan

```rs
cargo nextest run --release
```

---

_Label `red-knot` added by @sharkdp on 2024-11-26 09:34_

---

_Review requested from @carljm by @sharkdp on 2024-11-26 09:34_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-26 09:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-26 09:34_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/matcher.rs`:189 on 2024-11-26 09:37_

This uses a regex instead of something like `if ty.starts_with("@Todo(") { "@Todo" } else { ty }` or similar, because there might be more complicated types like `@Todo(…) | str`.

---

_@sharkdp reviewed on 2024-11-26 09:37_

---

_@sharkdp reviewed on 2024-11-26 09:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:5992 on 2024-11-26 09:39_

An alternative here would be to rename `assert_scope_ty` to `assert_scope_ty_str`, and then introduce a new helper to directly assert on the actual `Type`, not its string representation. I would hope that this test can be migrated to an mdtest sooner or later (#13696).

---

_@MichaReiser reviewed on 2024-11-26 09:41_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:187 on 2024-11-26 09:41_

We should use a static `Regex` to avoid recompiling it for every matched assertion

---

_@sharkdp reviewed on 2024-11-26 09:42_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/matcher.rs`:187 on 2024-11-26 09:42_

I was just about to write that I didn't bother doing that because it's just a test and we only run this when running tests in release mode, but okay :smile: 

---

_Comment by @github-actions[bot] on 2024-11-26 09:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2024-11-26 09:42_

This looks good to me. Considering that it's somewhat easy to introduce a new failure in the future, would you mind adding a `test` step to the release-job running on main

https://github.com/astral-sh/ruff/blob/66abef433b52e556c2d1b3332e7fd35fe278b98e/.github/workflows/ci.yaml#L212-L226

---

_@MichaReiser approved on 2024-11-26 14:38_

---

_Merged by @sharkdp on 2024-11-26 14:40_

---

_Closed by @sharkdp on 2024-11-26 14:40_

---

_Branch deleted on 2024-11-26 14:40_

---
