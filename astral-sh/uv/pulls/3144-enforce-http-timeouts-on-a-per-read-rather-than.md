```yaml
number: 3144
title: Enforce HTTP timeouts on a per-read (rather than per-request) basis
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-04-19T20:05:49Z
updated_at: 2024-04-19T21:14:01Z
url: https://github.com/astral-sh/uv/pull/3144
synced_at: 2026-01-12T16:05:27Z
```

# Enforce HTTP timeouts on a per-read (rather than per-request) basis

---

_@charliermarsh_

## Summary

This leverages the new `read_timeout` property, which ensures that (like pip) our timeout is not applied to the _entire_ request, but rather, to each individual read operation.

Closes: #1921.

See: #1912.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-19 20:05_

---

_Marked ready for review by @charliermarsh on 2024-04-19 20:05_

---

_Label `enhancement` added by @charliermarsh on 2024-04-19 20:06_

---

_Renamed from "Update reqwest to v0.12.4" to "Enforce HTTP timeouts on a per-read (rather than per-request) basis" by @charliermarsh on 2024-04-19 20:06_

---

_@zanieb approved on 2024-04-19 20:40_

---

_Comment by @zanieb on 2024-04-19 20:40_

I'd say this closes #1921 

---

_Merged by @charliermarsh on 2024-04-19 20:49_

---

_Closed by @charliermarsh on 2024-04-19 20:49_

---

_Branch deleted on 2024-04-19 20:49_

---

_Comment by @notatallshaw on 2024-04-19 21:14_

Probably fixes https://github.com/astral-sh/uv/issues/2833 also

---
