```yaml
number: 4789
title: Use cached environments in PEP 723 execution
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - preview
assignees: []
merged: true
base: main
head: charlie/cache-pep723-script
created_at: 2024-07-03T19:29:31Z
updated_at: 2024-07-05T21:00:49Z
url: https://github.com/astral-sh/uv/pull/4789
synced_at: 2026-01-10T13:48:28Z
```

# Use cached environments in PEP 723 execution

---

_Pull request opened by @charliermarsh on 2024-07-03 19:29_

## Summary

This seems like another good candidate for environment caching. If you run a script repeatedly, we can just use the existing cached environment.


---

_Label `performance` added by @charliermarsh on 2024-07-03 19:29_

---

_Label `preview` added by @charliermarsh on 2024-07-03 19:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-03 21:17_

---

_Review requested from @konstin by @charliermarsh on 2024-07-03 21:17_

---

_@konstin approved on 2024-07-04 08:50_

We should think about a plan for cached environment garbage collection

---

_@zanieb approved on 2024-07-04 15:36_

---

_Merged by @charliermarsh on 2024-07-04 17:23_

---

_Closed by @charliermarsh on 2024-07-04 17:23_

---

_Branch deleted on 2024-07-04 17:23_

---

_@Peiffap reviewed on 2024-07-04 22:55_

---

_Review comment by @Peiffap on `crates/uv/src/commands/project/environment.rs`:130 on 2024-07-04 22:55_

This reintroduced `EphemeralEnvironment` after you changed it in #4804. There's a few other places where the comments still need to be changed but given that the file is littered with TODOs I'll just comment here rather than open a PR and create merge conflicts.

---

_@charliermarsh reviewed on 2024-07-05 21:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/environment.rs`:130 on 2024-07-05 21:00_

Thank you!

---
