```yaml
number: 17282
title: "[syntax-errors] Extend annotation checks to `await`"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-await-in-annotations
created_at: 2025-04-07T16:55:23Z
updated_at: 2025-04-08T12:55:45Z
url: https://github.com/astral-sh/ruff/pull/17282
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Extend annotation checks to `await`

---

_Pull request opened by @ntBre on 2025-04-07 16:55_

Summary
--

This PR extends the changes in #17101 to include `await` in the same positions.

I also renamed the `valid_annotation_function` test to include `_py313` and explicitly passed a Python version to contrast it with the `_py314` version.

Test Plan
--

New test cases added to existing files.


---

_Label `rule` added by @ntBre on 2025-04-07 16:55_

---

_Label `preview` added by @ntBre on 2025-04-07 16:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-07 16:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-07 16:55_

---

_Comment by @github-actions[bot] on 2025-04-07 17:19_

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

_@MichaReiser approved on 2025-04-08 06:30_

---

_Merged by @ntBre on 2025-04-08 12:55_

---

_Closed by @ntBre on 2025-04-08 12:55_

---

_Branch deleted on 2025-04-08 12:55_

---
