```yaml
number: 8986
title: "Publish: Ignore non-matching files"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/ignore-non-matching-publish
created_at: 2024-11-10T11:09:17Z
updated_at: 2024-11-13T11:58:30Z
url: https://github.com/astral-sh/uv/pull/8986
synced_at: 2026-01-10T12:00:00Z
```

# Publish: Ignore non-matching files

---

_Pull request opened by @konstin on 2024-11-10 11:09_

Fixes #8944


---

_Label `bug` added by @konstin on 2024-11-10 11:09_

---

_@charliermarsh approved on 2024-11-10 16:58_

---

_@zanieb approved on 2024-11-11 15:14_

---

_Comment by @charliermarsh on 2024-11-11 17:55_

Tempted to say we should still error if we see something with a supported extension but an invalid filename (like `foo.whl`), or at least warn, but not blocking feedback.

---

_Comment by @konstin on 2024-11-13 11:58_

Good idea, added

---

_Merged by @konstin on 2024-11-13 11:58_

---

_Closed by @konstin on 2024-11-13 11:58_

---

_Branch deleted on 2024-11-13 11:58_

---
