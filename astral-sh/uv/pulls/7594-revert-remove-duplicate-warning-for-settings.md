```yaml
number: 7594
title: "Revert \"Remove duplicate warning for settings discovery errors (#7384)\""
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - tracing
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-09-20T17:42:08Z
updated_at: 2024-09-20T18:18:41Z
url: https://github.com/astral-sh/uv/pull/7594
synced_at: 2026-01-12T16:07:54Z
```

# Revert "Remove duplicate warning for settings discovery errors (#7384)"

---

_@charliermarsh_

## Summary

This reverts commit 3060fd22c00c3f5ca96374747600c8b4bab6a896.

These are now _never_ shown to users, because `tracing` isn't set up at that point. I'm going to try and improve the solution more holistically, but this is better than the status quo.

Closes https://github.com/astral-sh/uv/issues/7573.


---

_Label `bug` added by @charliermarsh on 2024-09-20 17:42_

---

_Label `tracing` added by @charliermarsh on 2024-09-20 17:42_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-20 17:42_

---

_@zanieb approved on 2024-09-20 18:02_

---

_Merged by @charliermarsh on 2024-09-20 18:18_

---

_Closed by @charliermarsh on 2024-09-20 18:18_

---

_Branch deleted on 2024-09-20 18:18_

---
