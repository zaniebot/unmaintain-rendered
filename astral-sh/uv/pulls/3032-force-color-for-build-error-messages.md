```yaml
number: 3032
title: Force color for build error messages
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/force-color-for-builds
created_at: 2024-04-15T09:05:02Z
updated_at: 2024-04-15T16:12:55Z
url: https://github.com/astral-sh/uv/pull/3032
synced_at: 2026-01-10T14:43:31Z
```

# Force color for build error messages

---

_Pull request opened by @konstin on 2024-04-15 09:05_

Since we're using anstream's strip stream, we can force color output from child processes and strip them when we redirect to a file

Before:

![image](https://github.com/astral-sh/uv/assets/6826232/ce8aafe9-687c-4c4a-970a-22abd660bc71)

After:

![image](https://github.com/astral-sh/uv/assets/6826232/bacedf1c-2462-4947-bd2f-393476a8031b)

Redirecting to a file correctly strips color codes.

---

_Label `enhancement` added by @konstin on 2024-04-15 09:05_

---

_Label `enhancement` removed by @konstin on 2024-04-15 09:07_

---

_Label `error messages` added by @konstin on 2024-04-15 09:07_

---

_@charliermarsh approved on 2024-04-15 14:45_

Seems reasonable

---

_Merged by @konstin on 2024-04-15 16:12_

---

_Closed by @konstin on 2024-04-15 16:12_

---

_Branch deleted on 2024-04-15 16:12_

---
