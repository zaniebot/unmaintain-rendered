```yaml
number: 2923
title: "feat: convert linehaul tests to use snapshots"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: linehaul-snapshot
created_at: 2024-04-09T01:17:56Z
updated_at: 2024-04-12T01:27:43Z
url: https://github.com/astral-sh/uv/pull/2923
synced_at: 2026-01-10T14:43:31Z
```

# feat: convert linehaul tests to use snapshots

---

_Pull request opened by @samypr100 on 2024-04-09 01:17_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #2564

## Test Plan

1. Changed existing linehaul tests to leverage insta.
2. Ran tests in various linux distros (Debian, Ubuntu, Centos, Fedora, Alpine) to ensure they also pass locally again.


---

_Marked ready for review by @samypr100 on 2024-04-09 01:23_

---

_Review requested from @zanieb by @konstin on 2024-04-09 08:50_

---

_@charliermarsh reviewed on 2024-04-10 17:52_

---

_Review comment by @charliermarsh on `crates/uv-client/tests/user_agent_version.rs`:198 on 2024-04-10 17:52_

Can we use inline snapshots here, rather than the separate JSON files? They're pretty small.

---

_@samypr100 reviewed on 2024-04-10 23:26_

---

_Review comment by @samypr100 on `crates/uv-client/tests/user_agent_version.rs`:198 on 2024-04-10 23:26_

Is this what you had in mind? c55686c787ae1c3020b29daf2683e5da13fd5f3b

---

_Label `internal` added by @konstin on 2024-04-11 09:29_

---

_@konstin approved on 2024-04-11 09:29_

Thanks

---

_Merged by @konstin on 2024-04-11 09:41_

---

_Closed by @konstin on 2024-04-11 09:41_

---

_Branch deleted on 2024-04-12 01:27_

---
