```yaml
number: 769
title: "Update scenario tests to include `requires-python` coverage"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/requires-python-scenarios
created_at: 2024-01-04T04:01:56Z
updated_at: 2024-01-04T20:15:14Z
url: https://github.com/astral-sh/uv/pull/769
synced_at: 2026-01-10T15:44:44Z
```

# Update scenario tests to include `requires-python` coverage

---

_Pull request opened by @zanieb on 2024-01-04 04:01_

Includes creating a virtual env with the relevant environment python version.

Scenarios added in https://github.com/zanieb/packse/pull/55


---

_@konstin reviewed on 2024-01-04 11:04_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_install_scenarios.rs`:529 on 2024-01-04 11:04_

I think eventually we need to special case this or at least add a hint what python version is currently used.

---

_Marked ready for review by @zanieb on 2024-01-04 17:17_

---

_Comment by @zanieb on 2024-01-04 19:17_

Yeesh this will require populating Python 3.7 - 3.11 in CI

Edit: Okay that was actually really easy

---

_Review requested from @charliermarsh by @zanieb on 2024-01-04 19:52_

---

_@charliermarsh approved on 2024-01-04 19:54_

---

_Merged by @zanieb on 2024-01-04 20:15_

---

_Closed by @zanieb on 2024-01-04 20:15_

---

_Branch deleted on 2024-01-04 20:15_

---
