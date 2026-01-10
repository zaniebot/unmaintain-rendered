```yaml
number: 20600
title: "[`ruff`] Do not flag `%r` + `repr()` combinations (`RUF065`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20583
created_at: 2025-09-27T04:59:41Z
updated_at: 2025-09-30T19:53:30Z
url: https://github.com/astral-sh/ruff/pull/20600
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Do not flag `%r` + `repr()` combinations (`RUF065`)

---

_Pull request opened by @danparizher on 2025-09-27 04:59_

## Summary

Fixes the first part of #20583


---

_Comment by @github-actions[bot] on 2025-09-27 05:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-09-30 19:47_

---

_Label `preview` added by @ntBre on 2025-09-30 19:47_

---

_@ntBre approved on 2025-09-30 19:49_

Thanks! I think it made sense to separate this from adding support for the other combinations in #20583.

I'll just unlink that issue from this to avoid closing it, and we can follow up with a separate PR expanding the rule.

---

_Merged by @ntBre on 2025-09-30 19:49_

---

_Closed by @ntBre on 2025-09-30 19:49_

---

_Branch deleted on 2025-09-30 19:53_

---
