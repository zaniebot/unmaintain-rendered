```yaml
number: 12206
title: Avoid creating duplicate directory entries in built wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/dupe
created_at: 2025-03-16T23:39:09Z
updated_at: 2025-03-16T23:48:36Z
url: https://github.com/astral-sh/uv/pull/12206
synced_at: 2026-01-12T16:10:10Z
```

# Avoid creating duplicate directory entries in built wheels

---

_@charliermarsh_

## Summary

This is a bug in the build backend revealed via https://github.com/astral-sh/uv/pull/12196. (By upgrading, `zip` now errors on duplicate entries.)


---

_Label `bug` added by @charliermarsh on 2025-03-16 23:39_

---

_Label `preview` added by @charliermarsh on 2025-03-16 23:39_

---

_Marked ready for review by @charliermarsh on 2025-03-16 23:39_

---

_Merged by @charliermarsh on 2025-03-16 23:48_

---

_Closed by @charliermarsh on 2025-03-16 23:48_

---

_Branch deleted on 2025-03-16 23:48_

---
