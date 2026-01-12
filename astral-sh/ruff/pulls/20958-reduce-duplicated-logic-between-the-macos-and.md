```yaml
number: 20958
title: Reduce duplicated logic between the macOS and Windows CI jobs
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/dry-ci
created_at: 2025-10-18T15:26:41Z
updated_at: 2025-10-18T17:00:58Z
url: https://github.com/astral-sh/ruff/pull/20958
synced_at: 2026-01-12T15:57:13Z
```

# Reduce duplicated logic between the macOS and Windows CI jobs

---

_@AlexWaygood_

## Summary

These CI jobs have almost identical logic in them. We can just use a matrix for the runner OS, to avoid writing it all out twice (which is bug-prone). Neither is marked as being required for (auto-)merge to succeed.

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-18 15:26_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:361 on 2025-10-18 15:27_

This issue has long-since been closed, so there's no need to set this environment variable anymore

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:380 on 2025-10-18 15:27_

This action is a no-op on both Windows and macOS, so this step was redundant: https://github.com/rui314/setup-mold/blob/725a8794d15fc7563f59595bd9556495c0564878/action.yml#L20

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:652 on 2025-10-18 15:28_

the 5-minute timeout isn't long enough on forks, where a free GitHub runner will be used instead of a depot runner

---

_@AlexWaygood reviewed on 2025-10-18 15:28_

---

_Marked ready for review by @AlexWaygood on 2025-10-18 15:28_

---

_Comment by @github-actions[bot] on 2025-10-18 15:43_

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

_@sharkdp approved on 2025-10-18 16:59_

Thank you

---

_Merged by @AlexWaygood on 2025-10-18 17:00_

---

_Closed by @AlexWaygood on 2025-10-18 17:00_

---

_Branch deleted on 2025-10-18 17:00_

---
