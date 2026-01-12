```yaml
number: 16206
title: "Allow missing `Scripts` directory"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: konsti/scripts-may-be-missing
created_at: 2025-10-09T12:46:38Z
updated_at: 2025-10-09T14:34:42Z
url: https://github.com/astral-sh/uv/pull/16206
synced_at: 2026-01-12T16:12:10Z
```

# Allow missing `Scripts` directory

---

_@konstin_

With the new Python install manager, the `Scripts` directory reported by Python may not exist.

See https://github.com/astral-sh/uv/pull/16205 for a failing CI run: https://github.com/astral-sh/uv/actions/runs/18377230241/job/52354460636?pr=16205#step:4:15

Fixes https://github.com/astral-sh/uv/issues/16204

---

_Label `bug` added by @konstin on 2025-10-09 12:46_

---

_Label `windows` added by @konstin on 2025-10-09 12:46_

---

_@zanieb approved on 2025-10-09 13:36_

---

_Comment by @konstin on 2025-10-09 13:50_

(Ready to merge, just waiting on GitHub API outage so we can see the integration test job pass again)

---

_Merged by @konstin on 2025-10-09 14:34_

---

_Closed by @konstin on 2025-10-09 14:34_

---

_Branch deleted on 2025-10-09 14:34_

---
