```yaml
number: 19562
title: "Add `TextEmitter::with_color` and disable colors in `unreadable_files` test"
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/fix-e902-colors
created_at: 2025-07-25T19:37:27Z
updated_at: 2025-07-25T19:47:51Z
url: https://github.com/astral-sh/ruff/pull/19562
synced_at: 2026-01-12T15:56:42Z
```

# Add `TextEmitter::with_color` and disable colors in `unreadable_files` test

---

_@ntBre_

Summary
--

I looked at other uses of `TextEmitter`, and I think this should be the only one affected by this. The other integration tests must work properly since they're run with `assert_cmd_snapshot!`, which I assume triggers the `SHOULD_COLORIZE` case, and the `cfg!(test)` check will work for uses in `ruff_linter`.

https://github.com/astral-sh/ruff/blob/4a4dc38b5b5055601dcb8da4ad07720e47d451fa/crates/ruff_linter/src/message/text.rs#L36-L44

Alternatively, we could probably move this to a CLI test instead.

Test Plan
--

`cargo test -p ruff`, which was failing on `main` with color codes in the output before this

---

_Label `testing` added by @ntBre on 2025-07-25 19:37_

---

_@MichaReiser approved on 2025-07-25 19:41_

---

_Comment by @github-actions[bot] on 2025-07-25 19:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-07-25 19:47_

---

_Closed by @ntBre on 2025-07-25 19:47_

---

_Branch deleted on 2025-07-25 19:47_

---
