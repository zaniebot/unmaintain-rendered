```yaml
number: 11266
title: "`ruff server`: Editor settings are used by default if no file-based configuration exists"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/merge-fallback-settings
created_at: 2024-05-03T17:35:09Z
updated_at: 2024-05-04T17:52:02Z
url: https://github.com/astral-sh/ruff/pull/11266
synced_at: 2026-01-10T22:37:02Z
```

# `ruff server`: Editor settings are used by default if no file-based configuration exists

---

_Pull request opened by @snowsignal on 2024-05-03 17:35_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11258.

This PR fixes the settings resolver to match the expected behavior when file-based configuration is not available.

## Test Plan

In a workspace with no file-based configuration, set a setting in your editor and confirm that this setting is used instead of the default.


---

_Label `bug` added by @snowsignal on 2024-05-03 17:35_

---

_Label `server` added by @snowsignal on 2024-05-03 17:35_

---

_Comment by @github-actions[bot] on 2024-05-03 17:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-04 13:28_

---

_Merged by @snowsignal on 2024-05-04 17:52_

---

_Closed by @snowsignal on 2024-05-04 17:52_

---

_Branch deleted on 2024-05-04 17:52_

---
