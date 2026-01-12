```yaml
number: 12313
title: Fail with specific error message when no password on auth always
type: pull_request
state: merged
author: jtfmumm
labels:
  - error messages
assignees: []
merged: true
base: main
head: jtfm/fail-on-username-no-password
created_at: 2025-03-19T15:17:16Z
updated_at: 2025-03-19T16:43:46Z
url: https://github.com/astral-sh/uv/pull/12313
synced_at: 2026-01-12T16:10:13Z
```

# Fail with specific error message when no password on auth always

---

_@jtfmumm_

This addresses a small part of #12280, namely when you have `authenticate` set to `always`, it will output a distinct error message for the case where you have a username but are missing a password.


---

_@zanieb approved on 2025-03-19 16:40_

---

_Label `error messages` added by @zanieb on 2025-03-19 16:40_

---

_Comment by @zanieb on 2025-03-19 16:41_

(Please label PRs so they're categorized in the changelog)

---

_Merged by @jtfmumm on 2025-03-19 16:43_

---

_Closed by @jtfmumm on 2025-03-19 16:43_

---

_Branch deleted on 2025-03-19 16:43_

---
