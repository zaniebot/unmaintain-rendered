```yaml
number: 19818
title: "[`isort`] Handle multiple continuation lines after module docstring (`I002`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19815
created_at: 2025-08-08T03:23:57Z
updated_at: 2025-08-15T21:41:46Z
url: https://github.com/astral-sh/ruff/pull/19818
synced_at: 2026-01-12T15:56:48Z
```

# [`isort`] Handle multiple continuation lines after module docstring (`I002`)

---

_@danparizher_

## Summary

Fixes #19815


---

_Comment by @github-actions[bot] on 2025-08-08 03:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-08 20:07_

---

_Label `bug` added by @ntBre on 2025-08-14 17:24_

---

_Label `fixes` added by @ntBre on 2025-08-14 17:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/insertion.rs`:391 on 2025-08-14 17:27_

Would you mind adding an I002 test for this too, like in #19505? That'll give a nice snapshot in addition to this assertion.

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/insertion.rs`:66 on 2025-08-14 17:31_

Do we need to change this to escape the `\` here? I think it was okay since we're in a comment.

I would probably just change the initial `If` in this comment to `While` instead of adding another sentence, much like the code change 😄 

---

_@ntBre reviewed on 2025-08-14 17:32_

Thanks, this looks right to me. I think a snapshot test would be helpful like in the last PR, though.

---

_Review requested from @ntBre by @danparizher on 2025-08-15 20:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/insertion.rs`:68 on 2025-08-15 21:03_

I think this part is basically redundant now.

```suggestion
            // additional rows to prevent inserting in the same logical line.
```

---

_@ntBre approved on 2025-08-15 21:06_

Thank you!

---

_Renamed from "[`isort`] Fix `I002`: handle multiple continuation backslashes after module docstring in `start_of_file` to avoid syntax errors" to "[`isort`] Handle multiple continuation lines after module docstring (`I002`)" by @ntBre on 2025-08-15 21:07_

---

_Merged by @ntBre on 2025-08-15 21:17_

---

_Closed by @ntBre on 2025-08-15 21:17_

---

_Branch deleted on 2025-08-15 21:41_

---
