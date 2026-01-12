```yaml
number: 3714
title: Move maturin test coverage into CI
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/maturin-test
created_at: 2024-05-21T18:33:35Z
updated_at: 2024-05-21T19:17:49Z
url: https://github.com/astral-sh/uv/pull/3714
synced_at: 2026-01-12T16:05:49Z
```

# Move maturin test coverage into CI

---

_@zanieb_

This test can take over 60s to run, which is too much for a unit test. We'll run it in a separate CI job to retain coverage.

---

_Label `testing` added by @zanieb on 2024-05-21 18:33_

---

_Marked ready for review by @zanieb on 2024-05-21 18:46_

---

_Review requested from @konstin by @zanieb on 2024-05-21 18:46_

---

_Review requested from @ibraheemdev by @zanieb on 2024-05-21 18:46_

---

_Comment by @charliermarsh on 2024-05-21 18:56_

Can we just remove this entirely? Seems like a lot of complexity to have an maintain job just for this.

---

_Comment by @zanieb on 2024-05-21 19:08_

I don't feel strongly, we could remove it. Why did @konstin think coverage was important?

---

_Comment by @zanieb on 2024-05-21 19:10_

My goal here is immediately unblock the rest of my PRs which are failing due to this test. It's trivial to remove after this merges so I have a preference to merge this as-is and consider dropping coverage separately.

---

_Label `internal` added by @zanieb on 2024-05-21 19:10_

---

_Comment by @charliermarsh on 2024-05-21 19:13_

Ok, feel free to merge, I'll remove in a separate PR then. Bias towards action :)

---

_Merged by @zanieb on 2024-05-21 19:17_

---

_Closed by @zanieb on 2024-05-21 19:17_

---

_Branch deleted on 2024-05-21 19:17_

---
