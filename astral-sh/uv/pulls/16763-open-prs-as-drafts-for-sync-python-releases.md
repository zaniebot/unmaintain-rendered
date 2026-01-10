```yaml
number: 16763
title: Open PRs as drafts for sync-python-releases
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/draft-bot-pr
created_at: 2025-11-17T20:43:00Z
updated_at: 2025-11-17T21:04:12Z
url: https://github.com/astral-sh/uv/pull/16763
synced_at: 2026-01-10T05:58:11Z
```

# Open PRs as drafts for sync-python-releases

---

_Pull request opened by @woodruffw on 2025-11-17 20:43_

## Summary

This is a little goofy, but it saves us a click: when automation PRs are opened as drafts, they don't need to be cycled through closed/opened to force the CI to run. Instead, once undrafted the CI runs.

See #16505 for an example of the closed/opened cycle hack this avoids.

## Test Plan

No functional changes besides CI automation.

---

_Review requested from @charliermarsh by @woodruffw on 2025-11-17 20:43_

---

_Review requested from @zanieb by @woodruffw on 2025-11-17 20:43_

---

_Assigned to @woodruffw by @woodruffw on 2025-11-17 20:43_

---

_Renamed from "chore(ci): open PRs as drafts for sync-python-releases" to "Open PRs as drafts for sync-python-releases" by @woodruffw on 2025-11-17 20:43_

---

_Label `internal` added by @woodruffw on 2025-11-17 20:43_

---

_@zanieb approved on 2025-11-17 21:04_

ty

---

_Merged by @zanieb on 2025-11-17 21:04_

---

_Closed by @zanieb on 2025-11-17 21:04_

---

_Branch deleted on 2025-11-17 21:04_

---
