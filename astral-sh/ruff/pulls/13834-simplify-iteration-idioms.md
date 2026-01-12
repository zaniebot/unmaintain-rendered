```yaml
number: 13834
title: Simplify iteration idioms
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: msrv-cleanup
created_at: 2024-10-20T21:07:48Z
updated_at: 2024-10-21T17:34:59Z
url: https://github.com/astral-sh/ruff/pull/13834
synced_at: 2026-01-12T15:55:45Z
```

# Simplify iteration idioms

---

_@AlexWaygood_

## Summary

This PR removes unnecessary uses of `.as_ref()`, `.iter()`, `&**` and similar, mostly in situations when iterating over variables. Many of these changes are only possible following #13826, when we bumped our MSRV to 1.80: several [useful implementations](https://doc.rust-lang.org/std/boxed/struct.Box.html#impl-IntoIterator-for-%26Box%3C%5BI%5D,+A%3E) on `&Box<[T]>` were only stabilised in Rust 1.80. Some of these changes we could have done earlier, however.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-10-20 21:07_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-20 21:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-20 21:07_

---

_@MichaReiser approved on 2024-10-20 21:22_

Nice

---

_Merged by @AlexWaygood on 2024-10-20 21:25_

---

_Closed by @AlexWaygood on 2024-10-20 21:25_

---

_Branch deleted on 2024-10-20 21:25_

---

_Comment by @github-actions[bot] on 2024-10-20 21:26_

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

_Comment by @carljm on 2024-10-21 17:31_

Nice! I'm curious how you found these. Was it just a grep for some common idioms and then test removing these and see if the compiler complained?

---

_Comment by @AlexWaygood on 2024-10-21 17:33_

> Nice! I'm curious how you found these. Was it just a grep for some common idioms and then test removing these and see if the compiler complained?

Yeah. I knew it was a thing we could do because we kept on trying to file red-knot PRs that directly iterated over boxed slices, and the MSRV check in CI was consistently complaining about them when all other checks passed...

---

_Comment by @AlexWaygood on 2024-10-21 17:34_

There are almost certainly some more in the Ruff repo that would be findable through some more sophisticated mechanism such as a Clippy lint 😄

---
