```yaml
number: 280
title: Don’t follow directory symlinks found while walking
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: symlink
created_at: 2022-09-29T03:21:03Z
updated_at: 2022-11-20T01:59:36Z
url: https://github.com/astral-sh/ruff/pull/280
synced_at: 2026-01-12T15:55:04Z
```

# Don’t follow directory symlinks found while walking

---

_@andersk_

A symlink to a directory probably points to a virtual environment or a dependency or some other directory that users don’t expect us to traverse, so ignore it unless it was passed directly on the command line.

We still follow symlinks to directories passed directly, as well as symlinks to files.

---

_Merged by @charliermarsh on 2022-09-29 19:10_

---

_Closed by @charliermarsh on 2022-09-29 19:10_

---

_Branch deleted on 2022-11-20 01:59_

---
