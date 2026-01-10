```yaml
number: 868
title: "Improve handling of \"full\" version ranges"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/full
created_at: 2024-01-10T05:34:45Z
updated_at: 2024-01-10T21:03:56Z
url: https://github.com/astral-sh/uv/pull/868
synced_at: 2026-01-10T15:39:02Z
```

# Improve handling of "full" version ranges

---

_Pull request opened by @zanieb on 2024-01-10 05:34_

Reduces the number of implementation branches handling `Range:full`, deferring it to `PackageRange`.
Improves some user-facing messages, e.g. saying `all versions of <package>` instead of `<package>*`.
Changes the member names of the `PackageRangeKind` enum — they were not very clear.

---

_Label `error messages` added by @zanieb on 2024-01-10 05:39_

---

_@charliermarsh reviewed on 2024-01-10 16:02_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_install_scenarios.rs`:292 on 2024-01-10 16:02_

I think it should be "all versions of c depend on" rather than "depends on".

---

_@zanieb reviewed on 2024-01-10 16:12_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:292 on 2024-01-10 16:12_

Oh gosh fair but.. painful to do.

I'll have to tackle this later as it'll require refactoring.

---

_Merged by @zanieb on 2024-01-10 21:03_

---

_Closed by @zanieb on 2024-01-10 21:03_

---

_Branch deleted on 2024-01-10 21:03_

---
