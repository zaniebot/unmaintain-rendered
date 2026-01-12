```yaml
number: 10527
title: Show expected and available ABI tags in resolver errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - no-build
assignees: []
merged: true
base: main
head: charlie/abi
created_at: 2025-01-11T23:22:58Z
updated_at: 2025-01-14T01:03:12Z
url: https://github.com/astral-sh/uv/pull/10527
synced_at: 2026-01-12T16:09:21Z
```

# Show expected and available ABI tags in resolver errors

---

_@charliermarsh_

## Summary

The idea here is to show both (1) an example of a compatible tag and (2) the tags that were available, whenever we fail to resolve due to an abscence of matching wheels.

Closes https://github.com/astral-sh/uv/issues/2777.


---

_@charliermarsh reviewed on 2025-01-11 23:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-11 23:26_

I think we may want to only show this on the most recent version (or the "best" incompatibility, so omit the ABI tags above).

---

_Label `no-build` added by @charliermarsh on 2025-01-12 00:58_

---

_Label `error messages` added by @charliermarsh on 2025-01-12 01:39_

---

_@zanieb reviewed on 2025-01-13 17:46_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/prioritized_distribution.rs`:187 on 2025-01-13 17:46_

not cyan?

---

_@zanieb reviewed on 2025-01-13 17:49_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:573 on 2025-01-13 17:49_

Looks like this comment should move or be expanded.

---

_@zanieb reviewed on 2025-01-13 18:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 18:00_

That seems reasonable to me, though I actually find it helpful to understand what's happening in this case.

---

_@zanieb reviewed on 2025-01-13 18:02_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 18:02_

In this example, I'm left wondering why I require `manylinux_2_17_x86_64`

---

_@zanieb reviewed on 2025-01-13 18:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:6761 on 2025-01-13 18:03_

There's no hint here that says we're solving for Python 3.12+, right? We might need to say that.

---

_@zanieb reviewed on 2025-01-13 18:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:4178 on 2025-01-13 18:04_

In this one, I would not find it obvious (as a user) that I'm solving for CPython but there are only wheels for GraalPython. This could be a good follow-up issue.

---

_@zanieb approved on 2025-01-13 18:04_

---

_@charliermarsh reviewed on 2025-01-13 23:41_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:41_

Should we say that the glibc version is too low...? Or, point out that `--python-platform linux` defaults to 2.17? That would be a pretty stellar error message but a lot more work.

---

_@zanieb reviewed on 2025-01-13 23:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:51_

I think the latter is what's needed here. I'd just open an issue to track it for now though.

---

_@charliermarsh reviewed on 2025-01-14 00:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-14 00:08_

The latter would be SICK

---

_@charliermarsh reviewed on 2025-01-14 00:55_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:6761 on 2025-01-14 00:55_

I think here and for ABI, we might want to say "CPython 3.12"

---

_Merged by @charliermarsh on 2025-01-14 01:03_

---

_Closed by @charliermarsh on 2025-01-14 01:03_

---

_Branch deleted on 2025-01-14 01:03_

---
