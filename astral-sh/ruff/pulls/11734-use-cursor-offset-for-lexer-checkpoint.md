```yaml
number: 11734
title: Use cursor offset for lexer checkpoint
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/cursor-offset
created_at: 2024-06-04T08:32:49Z
updated_at: 2024-06-04T08:52:15Z
url: https://github.com/astral-sh/ruff/pull/11734
synced_at: 2026-01-10T21:56:00Z
```

# Use cursor offset for lexer checkpoint

---

_Pull request opened by @dhruvmanila on 2024-06-04 08:32_

## Summary

This PR updates the lexer checkpoint to store the cursor offset instead of cloning the cursor itself. This reduces the size of `LexerCheckpoint` from 136 to 112 bytes and also removes the need for lifetime.

## Test Plan

`cargo insta test`

---

_Label `internal` added by @dhruvmanila on 2024-06-04 08:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-04 08:32_

---

_@dhruvmanila reviewed on 2024-06-04 08:33_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1376 on 2024-06-04 08:33_

We could use `Cursor::new(self.source[cursor_offset.to_usize()..]` but that would always set the previous character to `EOF`.

---

_@MichaReiser approved on 2024-06-04 08:39_

Nice!

---

_Merged by @dhruvmanila on 2024-06-04 08:43_

---

_Closed by @dhruvmanila on 2024-06-04 08:43_

---

_Branch deleted on 2024-06-04 08:43_

---

_Comment by @github-actions[bot] on 2024-06-04 08:52_

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
