```yaml
number: 12160
title: "Remove `demisto/content` from ecosystem checks"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
assignees: []
merged: true
base: main
head: dhruv/ecosystem
created_at: 2024-07-03T03:03:06Z
updated_at: 2024-07-03T08:39:38Z
url: https://github.com/astral-sh/ruff/pull/12160
synced_at: 2026-01-12T15:55:40Z
```

# Remove `demisto/content` from ecosystem checks

---

_@dhruvmanila_

## Summary

Follow-up to https://github.com/astral-sh/ruff/pull/12129 to remove the `demisto/content` from ecosystem checks. The previous PR removed it from the deprecated script which I didn't notice until recently.

## Test Plan

Ecosystem comment

---

_Label `ci` added by @dhruvmanila on 2024-07-03 03:04_

---

_Comment by @github-actions[bot] on 2024-07-03 03:28_

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

_@MichaReiser approved on 2024-07-03 06:09_

Should we delete the deprecated Script?

---

_Comment by @dhruvmanila on 2024-07-03 08:22_

> Should we delete the deprecated Script?

Yeah, I think that can be done.

---

_Merged by @dhruvmanila on 2024-07-03 08:25_

---

_Closed by @dhruvmanila on 2024-07-03 08:25_

---

_Branch deleted on 2024-07-03 08:25_

---
