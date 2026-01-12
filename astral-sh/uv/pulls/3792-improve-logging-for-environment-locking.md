```yaml
number: 3792
title: Improve logging for environment locking
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/lock
created_at: 2024-05-23T14:00:50Z
updated_at: 2024-05-23T14:27:08Z
url: https://github.com/astral-sh/uv/pull/3792
synced_at: 2026-01-12T16:05:51Z
```

# Improve logging for environment locking

---

_@zanieb_

```
DEBUG Acquired lock for `.venv`
```

instead of

```
DEBUG Trying to lock if free: .venv/.lock
```

At trace level, this includes the pre-lock message as well

```
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
```

We'll still display the lock file path when something goes wrong

---

_Label `tracing` added by @zanieb on 2024-05-23 14:00_

---

_@konstin approved on 2024-05-23 14:16_

---

_Merged by @zanieb on 2024-05-23 14:27_

---

_Closed by @zanieb on 2024-05-23 14:27_

---

_Branch deleted on 2024-05-23 14:27_

---
