```yaml
number: 17283
title: "[syntax-errors] Check annotations in annotated assignments"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-annotated-assignment-exprs
created_at: 2025-04-07T17:11:38Z
updated_at: 2025-04-08T12:56:28Z
url: https://github.com/astral-sh/ruff/pull/17283
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Check annotations in annotated assignments

---

_Pull request opened by @ntBre on 2025-04-07 17:11_

Summary
--

This PR extends the checks in #17101 and #17282 to annotated assignments after Python 3.13.

Currently stacked on #17282 to include `await`.

Test Plan
--

New inline tests. These are simpler than the other cases because there's no place to put generics.


---

_Label `rule` added by @ntBre on 2025-04-07 17:11_

---

_Label `preview` added by @ntBre on 2025-04-07 17:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-07 17:11_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-07 17:11_

---

_Comment by @github-actions[bot] on 2025-04-07 17:58_

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

_@MichaReiser approved on 2025-04-08 06:33_

---

_Merged by @ntBre on 2025-04-08 12:56_

---

_Closed by @ntBre on 2025-04-08 12:56_

---

_Branch deleted on 2025-04-08 12:56_

---
