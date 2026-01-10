```yaml
number: 14880
title: More typos found by codespell
type: pull_request
state: merged
author: DimitriPapadopoulos
labels:
  - internal
assignees: []
merged: true
base: main
head: codespell
created_at: 2024-12-09T16:15:50Z
updated_at: 2024-12-11T08:23:40Z
url: https://github.com/astral-sh/ruff/pull/14880
synced_at: 2026-01-10T20:42:27Z
```

# More typos found by codespell

---

_Pull request opened by @DimitriPapadopoulos on 2024-12-09 16:15_

## Summary

Fix more typos.

About `convertor` → `converter`, see for example https://thecontentauthority.com/blog/convertor-vs-converter.

## Test Plan

—

---

_Review requested from @carljm by @DimitriPapadopoulos on 2024-12-09 16:15_

---

_Review requested from @MichaReiser by @DimitriPapadopoulos on 2024-12-09 16:15_

---

_Review requested from @AlexWaygood by @DimitriPapadopoulos on 2024-12-09 16:15_

---

_Review requested from @sharkdp by @DimitriPapadopoulos on 2024-12-09 16:15_

---

_Comment by @github-actions[bot] on 2024-12-09 16:23_

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

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-09 16:24_

same comment as last time :-)

https://github.com/astral-sh/ruff/pull/14863#discussion_r1875617796

---

_@AlexWaygood reviewed on 2024-12-09 16:25_

Thanks!

---

_Label `internal` added by @AlexWaygood on 2024-12-09 17:20_

---

_@DimitriPapadopoulos reviewed on 2024-12-09 22:22_

---

_Review comment by @DimitriPapadopoulos on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-09 22:22_

There's no such word in the [OED](https://www.oed.com/search/dictionary/?q=disjointness). But then I guess the word is specifically used in a mathematical context.

---

_@AlexWaygood reviewed on 2024-12-09 22:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-09 22:47_

"disjointness" still sounds slightly better to me in this context. but whatever, I think the meaning is perfectly clear with either, and hopefully in the long run we'll just do the TODO anyway :-)

---

_Merged by @AlexWaygood on 2024-12-09 22:47_

---

_Closed by @AlexWaygood on 2024-12-09 22:47_

---

_@carljm reviewed on 2024-12-09 23:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-09 23:58_

I think "disjointedness" is a totally different word with a different meaning, and wouldn't have merged the PR with this change in it. But yeah, let's just do the TODO and get rid of the comment :)

---

_@DimitriPapadopoulos reviewed on 2024-12-10 09:25_

---

_Review comment by @DimitriPapadopoulos on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-10 09:25_

Indeed, `disjointness` appears to be used specifically in mathematics for disjoint sets.

---

_Branch deleted on 2024-12-10 09:26_

---

_Review comment by @DimitriPapadopoulos on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-12-11 08:23_

Reverted in #14906.

---

_@DimitriPapadopoulos reviewed on 2024-12-11 08:23_

---
