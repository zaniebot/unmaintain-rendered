```yaml
number: 4393
title: Treat mismatched directory and file urls as unsatisfied requirements
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/mismatch-url
created_at: 2024-06-18T18:06:34Z
updated_at: 2024-06-19T14:50:14Z
url: https://github.com/astral-sh/uv/pull/4393
synced_at: 2026-01-12T16:06:12Z
```

# Treat mismatched directory and file urls as unsatisfied requirements

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4391

---

_Label `bug` added by @zanieb on 2024-06-18 18:06_

---

_Review requested from @konstin by @zanieb on 2024-06-18 18:07_

---

_Comment by @zanieb on 2024-06-18 18:14_

Since the tests passed, this really looks like an oversight. I'll look into adding a test case. 

edit: Added in #4396 and rebased on top.

---

_Review requested from @BurntSushi by @zanieb on 2024-06-18 21:00_

---

_@charliermarsh approved on 2024-06-19 07:48_

Maybe this got changed at some point during testing and never reverted? Thanks.

---

_@BurntSushi approved on 2024-06-19 12:34_

---

_@konstin approved on 2024-06-19 14:30_

---

_Merged by @zanieb on 2024-06-19 14:50_

---

_Closed by @zanieb on 2024-06-19 14:50_

---

_Branch deleted on 2024-06-19 14:50_

---
