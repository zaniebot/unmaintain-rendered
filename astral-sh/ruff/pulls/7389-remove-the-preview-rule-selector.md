```yaml
number: 7389
title: "Remove the `PREVIEW` rule selector"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zanie/preview-selector
created_at: 2023-09-14T16:56:51Z
updated_at: 2023-09-14T17:32:05Z
url: https://github.com/astral-sh/ruff/pull/7389
synced_at: 2026-01-12T02:39:10Z
```

# Remove the `PREVIEW` rule selector

---

_Pull request opened by @zanieb on 2023-09-14 16:56_

The rule selector is not useful because `--select PREVIEW` only targets Ruff developers and `--ignore PREVIEW` has no effect due to its low specificity. We may restore it later if useful.

---

_@charliermarsh approved on 2023-09-14 17:01_

---

_Comment by @github-actions[bot] on 2023-09-14 17:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @zanieb on 2023-09-14 17:32_

---

_Closed by @zanieb on 2023-09-14 17:32_

---

_Branch deleted on 2023-09-14 17:32_

---

_Label `preview` added by @zanieb on 2023-09-14 17:32_

---
