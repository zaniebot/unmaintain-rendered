```yaml
number: 7318
title: Avoid installing transitive dev dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dev-transitive
created_at: 2024-09-12T01:29:10Z
updated_at: 2024-09-19T10:56:04Z
url: https://github.com/astral-sh/uv/pull/7318
synced_at: 2026-01-10T12:53:44Z
```

# Avoid installing transitive dev dependencies

---

_Pull request opened by @charliermarsh on 2024-09-12 01:29_

## Summary

This is arguably breaking, arguably a bug... Today, if project A depends on project B, and you install A with dev dependencies enabled, you also get B's dev dependencies. I think this is incorrect. Just like you shouldn't be importing B's dependencies from A, you shouldn't be using B's dev dependencies when developing on A.

Closes #7310.


---

_Label `bug` added by @charliermarsh on 2024-09-12 01:29_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-12 01:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-12 01:29_

---

_Review requested from @konstin by @charliermarsh on 2024-09-12 01:29_

---

_Marked ready for review by @charliermarsh on 2024-09-12 01:29_

---

_@BurntSushi approved on 2024-09-12 13:15_

Makes sense to me!

---

_Merged by @charliermarsh on 2024-09-12 13:20_

---

_Closed by @charliermarsh on 2024-09-12 13:20_

---

_Branch deleted on 2024-09-12 13:20_

---
