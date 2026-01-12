```yaml
number: 9332
title: Avoid subsequent index hint when no versions are available on the first index
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/index-hint
created_at: 2024-11-21T17:38:47Z
updated_at: 2025-03-14T01:09:10Z
url: https://github.com/astral-sh/uv/pull/9332
synced_at: 2026-01-12T16:08:45Z
```

# Avoid subsequent index hint when no versions are available on the first index

---

_@zanieb_

As reported in https://github.com/astral-sh/uv/issues/9331, this hint is misleading.

---

_Label `error messages` added by @zanieb on 2024-11-21 17:38_

---

_@charliermarsh reviewed on 2024-11-21 19:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:731 on 2024-11-21 19:56_

What is this testing for exactly? Sorry!

---

_@zanieb reviewed on 2024-11-21 19:58_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:731 on 2024-11-21 19:58_

An unbounded range, e.g., `foo` instead of `foo>=0.1`

---

_@charliermarsh reviewed on 2024-11-21 20:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:731 on 2024-11-21 20:06_

So if it's unbounded, does that mean we didn't find any versions at all?

---

_@zanieb reviewed on 2024-11-22 16:24_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:731 on 2024-11-22 16:24_

Yeah, see the error message in the linked issue. It doesn't make sense to say that a subsequent index may include the version you've requested.

---

_Comment by @charliermarsh on 2025-03-14 00:51_

@zanieb -- Ah I think I see a case where this could happen: https://test.pypi.org/simple/pandas/

---

_@charliermarsh approved on 2025-03-14 00:52_

---

_Comment by @charliermarsh on 2025-03-14 01:01_

I think we can merge this as-is (I modified the test to use a package that exists but without any versions), though the hint could be further improved.

---

_Merged by @charliermarsh on 2025-03-14 01:09_

---

_Closed by @charliermarsh on 2025-03-14 01:09_

---

_Branch deleted on 2025-03-14 01:09_

---
