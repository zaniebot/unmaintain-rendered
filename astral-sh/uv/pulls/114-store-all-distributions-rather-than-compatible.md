```yaml
number: 114
title: Store all distributions rather than compatible wheels
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/store-all
created_at: 2023-10-17T20:46:38Z
updated_at: 2023-10-17T21:09:32Z
url: https://github.com/astral-sh/uv/pull/114
synced_at: 2026-01-10T15:50:28Z
```

# Store all distributions rather than compatible wheels

---

_Pull request opened by @charliermarsh on 2023-10-17 20:46_

This PR reverts #109 which is actually a performance _regression_ since we need to iterate over a bunch of wheels that we could otherwise entirely ignore.

---

_Comment by @charliermarsh on 2023-10-17 20:47_

Probably a more optimal solution here in which we discard invalid files on the fly to avoid looking at them repeatedly.

---

_Merged by @charliermarsh on 2023-10-17 21:09_

---

_Closed by @charliermarsh on 2023-10-17 21:09_

---

_Branch deleted on 2023-10-17 21:09_

---
