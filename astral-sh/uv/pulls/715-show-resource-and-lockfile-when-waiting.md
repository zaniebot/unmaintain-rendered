```yaml
number: 715
title: Show resource and lockfile when waiting
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/lockfile-info
created_at: 2023-12-20T22:56:49Z
updated_at: 2023-12-20T23:05:50Z
url: https://github.com/astral-sh/uv/pull/715
synced_at: 2026-01-12T16:04:08Z
```

# Show resource and lockfile when waiting

---

_@konstin_

We lock git checkout directories and the virtualenv to avoid two puffin instances running in parallel changing files at the same time and leading to a broken state. When one instance is blocking another, we need to inform the user (why is the program hanging?) and also add some information for them to debug the situation.

The new messages will print

```
Waiting to acquire lock for /home/konsti/projects/puffin/.venv (lockfile: /home/konsti/projects/puffin/.venv/.lock)
```

or

```
Waiting to acquire lock for git+https://github.com/pydantic/pydantic-extra-types@0ce9f207a1e09a862287ab77512f0060c1625223 (lockfile: /home/konsti/projects/puffin/cache-all-kinds/git-v0/locks/f157fd329a506a34)
```

The messages aren't perfect but clear enough to see what the contention is and in the worst case to delete the lockfile.

Fixes #714

---

_@charliermarsh approved on 2023-12-20 23:00_

Nice, thanks

---

_Merged by @konstin on 2023-12-20 23:05_

---

_Closed by @konstin on 2023-12-20 23:05_

---

_Branch deleted on 2023-12-20 23:05_

---
