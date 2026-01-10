```yaml
number: 9551
title: Fix lock_requires_python_exact perf
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-lock_requires_python_exact
created_at: 2024-12-01T11:00:10Z
updated_at: 2024-12-04T14:17:22Z
url: https://github.com/astral-sh/uv/pull/9551
synced_at: 2026-01-10T12:00:00Z
```

# Fix lock_requires_python_exact perf

---

_Pull request opened by @konstin on 2024-12-01 11:00_

When running `lock_requires_python_exact`, we would download CPython 3.12.0 each time. By instead downloading CPython 3.13.0 ahead of time and passing it in, we speed the test up and avoid timeouts. Locally in pycharm, the test goes from 6.5s to 500ms.

---

_Label `internal` added by @konstin on 2024-12-01 11:00_

---

_Review requested from @charliermarsh by @konstin on 2024-12-01 11:00_

---

_Review requested from @zanieb by @konstin on 2024-12-01 11:00_

---

_Comment by @charliermarsh on 2024-12-01 12:45_

Somewhat annoying that we have to download a separate Python for this test, since that too has a cost.m. I'm wondering if we should just remove this or test it via a unit test or similar.

---

_Comment by @charliermarsh on 2024-12-01 12:45_

(Good catch.)

---

_Comment by @konstin on 2024-12-01 13:26_

Agree with it being annoying, but it's a `warn_user_once!`, so not sure how to unit-test this.

---

_Comment by @charliermarsh on 2024-12-01 13:43_

Should we just remove it?

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3793 on 2024-12-03 15:41_

This needs the `python-patch` feature if we're going to use a specific patch version

---

_@zanieb reviewed on 2024-12-03 15:41_

---

_@zanieb reviewed on 2024-12-03 15:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3793 on 2024-12-03 15:43_

(which will fix Windows)

---

_Comment by @zanieb on 2024-12-03 15:44_

I guess we could remove it? I don't have strong feelings.

> Somewhat annoying that we have to download a separate Python for this test, since that too has a cost.

The cost is pretty low, but yeah not great. We should at least switch to 3.13.0 since that should have the latest distribution size reductions that 3.12.0 might not?

---

_Comment by @konstin on 2024-12-03 15:46_

Let's go with 3.13.0, I'll update the PR

---

_Comment by @charliermarsh on 2024-12-03 15:46_

Yeah we also _already_ download 3.13.0, so that at least works for now.

---

_Merged by @charliermarsh on 2024-12-04 14:17_

---

_Closed by @charliermarsh on 2024-12-04 14:17_

---

_Branch deleted on 2024-12-04 14:17_

---
