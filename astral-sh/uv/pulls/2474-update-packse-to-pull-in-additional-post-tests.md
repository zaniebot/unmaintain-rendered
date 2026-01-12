```yaml
number: 2474
title: Update packse to pull in additional post tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/packse
created_at: 2024-03-15T14:24:03Z
updated_at: 2024-03-15T14:34:37Z
url: https://github.com/astral-sh/uv/pull/2474
synced_at: 2026-01-12T16:05:04Z
```

# Update packse to pull in additional post tests

---

_@charliermarsh_

_No description provided._

---

_@charliermarsh reviewed on 2024-03-15 14:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:2496 on 2024-03-15 14:25_

This is not totally ideal (though it is _correct_) because the user asked for `>1.2.3.post2`. I think it's fine though. (These are rare anyway.)

---

_Label `testing` added by @charliermarsh on 2024-03-15 14:29_

---

_@zanieb reviewed on 2024-03-15 14:31_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:2496 on 2024-03-15 14:31_

Oh interesting... thanks for adding the coverage. We can address later if it's a problem. Do you know why this happens? jw

---

_Merged by @charliermarsh on 2024-03-15 14:34_

---

_Closed by @charliermarsh on 2024-03-15 14:34_

---

_Branch deleted on 2024-03-15 14:34_

---

_@charliermarsh reviewed on 2024-03-15 14:34_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:2496 on 2024-03-15 14:34_

Yeah, when we see `post-greater-than-post-not-available-a>1.2.3.post2`, we internally rewrite it as `post-greater-than-post-not-available-a>=1.2.3.post3`, since that's the easiest way to represent the constraint.

---
