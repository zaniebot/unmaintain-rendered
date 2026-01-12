```yaml
number: 16473
title: "[syntax-errors] Limit `except*` range to `*`"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/except-star-range
created_at: 2025-03-03T14:39:46Z
updated_at: 2025-03-04T16:55:31Z
url: https://github.com/astral-sh/ruff/pull/16473
synced_at: 2026-01-12T15:55:55Z
```

# [syntax-errors] Limit `except*` range to `*`

---

_@ntBre_

Summary
--
This is a follow-up to #16446 to fix the diagnostic range to point to the `*` like `pyright` does (https://github.com/astral-sh/ruff/pull/16446#discussion_r1976900643).

Storing the range in the `ExceptClauseKind::Star` variant feels slightly awkward, but we don't store the star itself anywhere on the `ExceptHandler`. And we can't just take `ExceptHandler.start() + "except".text_len()` because this code appears to be valid:

```python
try: ...
except    *    Error: ...
```

Test Plan
--
Existing tests.


---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 14:39_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 14:39_

---

_Label `parser` added by @ntBre on 2025-03-03 14:45_

---

_Label `preview` added by @ntBre on 2025-03-03 14:45_

---

_Comment by @github-actions[bot] on 2025-03-03 14:48_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1493 on 2025-03-04 11:43_

What about `star_token_start`? I'd also directly use `self.current_token_range().start()` or provide a `self.current_token_start()` method as otherwise `node_start` reads a bit out of context with regards to what the variable is for.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1472 on 2025-03-04 11:45_

We can add some whitespaces around the star operator like `except  *    KeyError` to make sure that the highlighted range is still correct.

---

_@dhruvmanila approved on 2025-03-04 11:45_

---

_@ntBre reviewed on 2025-03-04 16:43_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:1493 on 2025-03-04 16:43_

Oh nice, I can actually just use the `current_token_range()` directly since I needed a range anyway!

---

_Merged by @ntBre on 2025-03-04 16:50_

---

_Closed by @ntBre on 2025-03-04 16:50_

---

_Branch deleted on 2025-03-04 16:50_

---
