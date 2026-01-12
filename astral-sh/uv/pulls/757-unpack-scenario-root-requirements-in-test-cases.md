```yaml
number: 757
title: Unpack scenario root requirements in test cases
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/packse-test-root
created_at: 2024-01-03T21:51:11Z
updated_at: 2024-01-03T23:31:31Z
url: https://github.com/astral-sh/uv/pull/757
synced_at: 2026-01-12T16:04:10Z
```

# Unpack scenario root requirements in test cases

---

_@zanieb_

As mentioned in #746, instead of just installing the scenario root we will unpack the root dependencies into the install command to allow better coverage of direct user requests with scenarios.

I added display of the package tree provided by each scenario.

Use a mustache template for iterative replacements.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-03 22:39_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_install_scenarios.rs`:119 on 2024-01-03 22:45_

Do the templates need safe mode turned off?

---

_@konstin approved on 2024-01-03 22:47_

---

_@zanieb reviewed on 2024-01-03 22:48_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:119 on 2024-01-03 22:48_

Ah thank you! Will fix that...

---

_Merged by @zanieb on 2024-01-03 23:31_

---

_Closed by @zanieb on 2024-01-03 23:31_

---

_Branch deleted on 2024-01-03 23:31_

---
