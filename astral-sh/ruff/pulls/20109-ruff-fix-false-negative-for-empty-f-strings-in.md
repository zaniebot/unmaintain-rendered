```yaml
number: 20109
title: "[`ruff`] Fix false negative for empty f-strings in `deque` calls (`RUF037`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20050
created_at: 2025-08-27T03:04:28Z
updated_at: 2025-08-28T21:25:12Z
url: https://github.com/astral-sh/ruff/pull/20109
synced_at: 2026-01-12T15:56:54Z
```

# [`ruff`] Fix false negative for empty f-strings in `deque` calls (`RUF037`)

---

_@danparizher_

## Summary

Fixes #20050


---

_Comment by @github-actions[bot] on 2025-08-27 03:16_

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

_Label `rule` added by @ntBre on 2025-08-28 20:02_

---

_Label `preview` added by @ntBre on 2025-08-28 20:02_

---

_@ntBre reviewed on 2025-08-28 20:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_literal_within_deque_call.rs`:106 on 2025-08-28 20:08_

Did you try an approach using [`is_empty_f_string`](https://github.com/astral-sh/ruff/blob/3927b0c9318f4b9b3c71127de5030aefb8390ed5/crates/ruff_python_ast/src/helpers.rs#L1409) as suggested on the issue? That seems a bit cleaner if it would accomplish the same thing.

---

_@danparizher reviewed on 2025-08-28 20:43_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_literal_within_deque_call.rs`:106 on 2025-08-28 20:43_

Didn't see that, thanks!

---

_Review requested from @ntBre by @danparizher on 2025-08-28 20:53_

---

_@ntBre approved on 2025-08-28 20:58_

Thanks!

---

_Merged by @ntBre on 2025-08-28 20:58_

---

_Closed by @ntBre on 2025-08-28 20:58_

---

_Branch deleted on 2025-08-28 21:25_

---
