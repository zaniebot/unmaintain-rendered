```yaml
number: 1067
title: Tweak summary message to include total error counts
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/summary
created_at: 2022-12-05T16:57:58Z
updated_at: 2022-12-05T21:12:13Z
url: https://github.com/astral-sh/ruff/pull/1067
synced_at: 2026-01-12T15:55:05Z
```

# Tweak summary message to include total error counts

---

_@charliermarsh_

We now print, e.g., `Found 3 error(s) (3 fixed, 0 remaining).`.

Resolves: #1061.


---

_Comment by @charliermarsh on 2022-12-05 17:55_

I think the one risk of confusion here is that if we autofix, and that leads to new errors (e.g., we remove a statement, which leads to an unused import), then the counts can be unintuitive.

---

_Merged by @charliermarsh on 2022-12-05 21:12_

---

_Closed by @charliermarsh on 2022-12-05 21:12_

---

_Branch deleted on 2022-12-05 21:12_

---
