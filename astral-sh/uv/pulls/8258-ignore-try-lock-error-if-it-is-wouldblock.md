```yaml
number: 8258
title: Ignore try lock error if it is WouldBlock
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-16T14:37:33Z
updated_at: 2024-10-16T19:15:48Z
url: https://github.com/astral-sh/uv/pull/8258
synced_at: 2026-01-10T12:54:06Z
```

# Ignore try lock error if it is WouldBlock

---

_Pull request opened by @j178 on 2024-10-16 14:37_

## Summary

Address a TODO comment, seems like we don't need `raw_os_err`?


---

_Review requested from @zanieb by @charliermarsh on 2024-10-16 15:35_

---

_@zanieb approved on 2024-10-16 16:57_

Oh cool, not really sure why I thought I needed the new feature.

Should we elevate this to `debug` then?

---

_Merged by @zanieb on 2024-10-16 19:15_

---

_Closed by @zanieb on 2024-10-16 19:15_

---
