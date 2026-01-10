```yaml
number: 14170
title: "Bump the test timeout from 90s -> 120s"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/t120
created_at: 2025-06-20T20:10:34Z
updated_at: 2025-07-02T13:39:59Z
url: https://github.com/astral-sh/uv/pull/14170
synced_at: 2026-01-10T06:53:01Z
```

# Bump the test timeout from 90s -> 120s

---

_Pull request opened by @zanieb on 2025-06-20 20:10_

In hopes of resolving https://github.com/astral-sh/uv/issues/14158

We should also see why the tests are so slow though.

---

_Label `internal` added by @zanieb on 2025-06-20 20:10_

---

_Review requested from @Gankra by @zanieb on 2025-06-20 20:10_

---

_Marked ready for review by @zanieb on 2025-06-20 20:11_

---

_@oconnor663 approved on 2025-06-20 20:27_

---

_Comment by @zanieb on 2025-06-20 20:48_

Well

```
     TIMEOUT [ 120.116s] uv::it pip_uninstall::uninstall_by_path
  stdout ───

    running 1 test
    test pip_uninstall::uninstall_by_path has been running for over 60 seconds

    (test timed out)

     TIMEOUT [ 120.130s] uv::it pip_uninstall::uninstall_duplicate_by_path
  stdout ───

    running 1 test
    test pip_uninstall::uninstall_duplicate_by_path has been running for over 60 seconds

    (test timed out)

error: test run failed
```

Another solution will be needed

---

_Comment by @zanieb on 2025-07-02 13:30_

I think this is worth it regardless, it'll reduce the margin of slowdown required to cause a CI failure.

---

_Merged by @zanieb on 2025-07-02 13:39_

---

_Closed by @zanieb on 2025-07-02 13:39_

---

_Branch deleted on 2025-07-02 13:39_

---
