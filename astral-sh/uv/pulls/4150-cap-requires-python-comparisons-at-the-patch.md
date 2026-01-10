```yaml
number: 4150
title: "Cap `Requires-Python` comparisons at the patch version"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2024-06-07T22:25:26Z
updated_at: 2024-06-10T13:21:07Z
url: https://github.com/astral-sh/uv/pull/4150
synced_at: 2026-01-10T13:54:02Z
```

# Cap `Requires-Python` comparisons at the patch version

---

_Pull request opened by @charliermarsh on 2024-06-07 22:25_

## Summary

See the long comment inline. I think this is debatable but probably right for now. The other options have their own problems, but there are a few alternate ideas in the comment.

Closes https://github.com/astral-sh/uv/issues/4132.


---

_Label `preview` added by @charliermarsh on 2024-06-07 22:25_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-07 22:25_

---

_Review requested from @konstin by @charliermarsh on 2024-06-07 22:25_

---

_@zanieb approved on 2024-06-07 23:16_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:1833 on 2024-06-07 23:17_

Is it worth adding a test case that explicitly uses `>=3.11.0b1` too?

---

_@zanieb reviewed on 2024-06-07 23:17_

---

_Merged by @charliermarsh on 2024-06-08 01:22_

---

_Closed by @charliermarsh on 2024-06-08 01:22_

---

_Branch deleted on 2024-06-08 01:22_

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:130 on 2024-06-10 06:17_

nit: This should be `==3.8.*`

---

_@konstin reviewed on 2024-06-10 06:17_

---

_@charliermarsh reviewed on 2024-06-10 13:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:130 on 2024-06-10 13:21_

Thank you

---
