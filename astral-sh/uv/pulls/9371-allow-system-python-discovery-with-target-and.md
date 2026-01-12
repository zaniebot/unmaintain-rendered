```yaml
number: 9371
title: "Allow system Python discovery with `--target` and `--prefix`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/target
created_at: 2024-11-22T20:55:24Z
updated_at: 2024-11-22T22:06:44Z
url: https://github.com/astral-sh/uv/pull/9371
synced_at: 2026-01-12T16:08:47Z
```

# Allow system Python discovery with `--target` and `--prefix`

---

_@charliermarsh_

## Summary

If we're installing with `--target` or `--prefix`, then it's not a mutable operation, so we should be allowed to discover system Pythons. I suspect this was hard to special-case in the past but is now trivial after @zanieb's various refactors.

Closes https://github.com/astral-sh/uv/issues/9356.


---

_Label `bug` added by @charliermarsh on 2024-11-22 20:55_

---

_@zanieb approved on 2024-11-22 21:00_

I think this seems reasonable, yeah. Should we add a unit test demonstrating this allows targeting an empty directory now?

---

_Merged by @charliermarsh on 2024-11-22 22:06_

---

_Closed by @charliermarsh on 2024-11-22 22:06_

---

_Branch deleted on 2024-11-22 22:06_

---
