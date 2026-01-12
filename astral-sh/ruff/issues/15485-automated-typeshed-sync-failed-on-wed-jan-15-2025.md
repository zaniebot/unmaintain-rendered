```yaml
number: 15485
title: Automated typeshed sync failed on Wed Jan 15 2025
type: issue
state: closed
author: github-actions
labels:
  - ci
assignees: []
created_at: 2025-01-15T05:09:05Z
updated_at: 2025-01-15T10:21:02Z
url: https://github.com/astral-sh/ruff/issues/15485
synced_at: 2026-01-12T15:54:54Z
```

# Automated typeshed sync failed on Wed Jan 15 2025

---

_@github-actions_

Runs are listed here: https://github.com/astral-sh/ruff/actions/workflows/sync_typeshed.yaml

---

_Comment by @dhruvmanila on 2025-01-15 05:11_

cc @AlexWaygood, I tried re-running but that failed as well. The specific workflow job is https://github.com/astral-sh/ruff/actions/runs/12778917699/job/35630616320

---

_Label `ci` added by @dhruvmanila on 2025-01-15 05:11_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-15 06:33_

---

_Comment by @sharkdp on 2025-01-15 10:01_

Looks like this is related to https://github.com/cli/cli/issues/10188 which has been fixed upstream in the meantime. I suggest we do one manual sync (#15492) and then simply wait for this runner update: https://github.com/actions/runner-images/pull/11377.

---

_Closed by @sharkdp on 2025-01-15 10:21_

---
