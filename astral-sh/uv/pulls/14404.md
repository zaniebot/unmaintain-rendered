```yaml
number: 14404
title: Make project and interpreter lock acquisition non-fatal
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/nf
created_at: 2025-07-02T01:19:26Z
updated_at: 2025-07-02T18:03:45Z
url: https://github.com/astral-sh/uv/pull/14404
synced_at: 2026-01-10T06:53:01Z
```

# Make project and interpreter lock acquisition non-fatal

---

_Pull request opened by @charliermarsh on 2025-07-02 01:19_

## Summary

If we fail to acquire a lock on an environment, uv shouldn't fail; we should just warn. In some cases, users run uv with read-only permissions for their projects, etc.

For now, I kept any locks acquired _in the cache_ as hard failures, since we always need write-access to the cache.

Closes https://github.com/astral-sh/uv/issues/14411.


---

_Review requested from @oconnor663 by @charliermarsh on 2025-07-02 01:19_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-02 01:19_

---

_Label `enhancement` added by @charliermarsh on 2025-07-02 01:19_

---

_Marked ready for review by @charliermarsh on 2025-07-02 01:19_

---

_@zanieb approved on 2025-07-02 03:19_

Should these be real user-facing warnings instead of tracing? It might be step too far to go from an error to only showing failures with `-v` though it might be nice to wait until we add a hint to _resolve_ the problem 

---

_Comment by @zanieb on 2025-07-02 03:20_

You probably could write a test for this, e.g., by setting `TMP` to a tmpdir without write permissions? It'd be nice to have coverage for when we add more messaging on how to resolve the problem.

---

_Comment by @charliermarsh on 2025-07-02 17:48_

Added a test, but per Discord, agreed to keep these non-user-facing for now.

---

_Comment by @charliermarsh on 2025-07-02 17:50_

(And verified that the test fails on main with the same trace as in https://github.com/astral-sh/uv/issues/14411.)

---

_Label `enhancement` removed by @charliermarsh on 2025-07-02 17:50_

---

_Label `bug` added by @charliermarsh on 2025-07-02 17:50_

---

_@zanieb approved on 2025-07-02 17:52_

---

_Merged by @charliermarsh on 2025-07-02 18:03_

---

_Closed by @charliermarsh on 2025-07-02 18:03_

---

_Branch deleted on 2025-07-02 18:03_

---
