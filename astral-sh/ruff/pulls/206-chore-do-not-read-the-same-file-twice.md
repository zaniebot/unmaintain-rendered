```yaml
number: 206
title: "chore: Do not read the same file twice"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/read-once
created_at: 2022-09-15T18:20:46Z
updated_at: 2022-09-15T20:14:17Z
url: https://github.com/astral-sh/ruff/pull/206
synced_at: 2026-01-12T05:48:45Z
```

# chore: Do not read the same file twice

---

_Pull request opened by @Stranger6667 on 2022-09-15 18:20_

I noticed that `ruff` reads the same file twice if there is no cache. This PR passes `contents` to `check_path`

---

_Comment by @charliermarsh on 2022-09-15 20:05_

Oh great catch! I think this was a regression (I split `check_path` into two methods to facilitate testing at some point.)

---

_Merged by @charliermarsh on 2022-09-15 20:05_

---

_Closed by @charliermarsh on 2022-09-15 20:05_

---

_Branch deleted on 2022-09-15 20:14_

---
