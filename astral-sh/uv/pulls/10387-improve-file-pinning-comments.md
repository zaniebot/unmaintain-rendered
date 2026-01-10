```yaml
number: 10387
title: Improve file pinning comments
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/file-pins-comment
created_at: 2025-01-08T10:36:01Z
updated_at: 2025-01-08T11:42:27Z
url: https://github.com/astral-sh/uv/pull/10387
synced_at: 2026-01-10T11:44:46Z
```

# Improve file pinning comments

---

_Pull request opened by @konstin on 2025-01-08 10:36_

Not ideal still but it captures the reason why the code is the way it is.

Ideally we should remove file pins as it exists now entirely, we're doing a expensive clone (it's noticeable in profiles) of a type that we can still retrieve later only to discard most of them after the resolution is done; But that's out of scope for now.

---

_Label `internal` added by @konstin on 2025-01-08 10:36_

---

_Merged by @konstin on 2025-01-08 11:42_

---

_Closed by @konstin on 2025-01-08 11:42_

---

_Branch deleted on 2025-01-08 11:42_

---
