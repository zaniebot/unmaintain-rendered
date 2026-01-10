```yaml
number: 790
title: Add pre-release scenarios
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/pre-releases
created_at: 2024-01-04T20:53:15Z
updated_at: 2024-01-05T03:10:44Z
url: https://github.com/astral-sh/uv/pull/790
synced_at: 2026-01-10T15:44:44Z
```

# Add pre-release scenarios

---

_Pull request opened by @zanieb on 2024-01-04 20:53_

Scenarios added in https://github.com/zanieb/packse/pull/58

---

_Comment by @zanieb on 2024-01-04 20:56_

I just noticed Konsti contributed some scenarios too. I'll get those merged in, might bump to that commit separately.

edit: done

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:170 on 2024-01-04 22:39_

Why no prerelease hint here?

---

_@zanieb reviewed on 2024-01-04 22:39_

---

_@charliermarsh reviewed on 2024-01-05 01:23_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_install_scenarios.rs`:170 on 2024-01-05 01:23_

We only show the hint if there's a pre-release marker somewhere (and the scenario doesn't have any packages that _request_ pre-release markers even transitively).

---

_@charliermarsh approved on 2024-01-05 01:23_

These are amazing.

---

_@zanieb reviewed on 2024-01-05 02:20_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:170 on 2024-01-05 02:20_

Ah okay thanks!

---

_@zanieb reviewed on 2024-01-05 03:06_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:788 on 2024-01-05 03:06_

Here's our hint case

---

_Merged by @zanieb on 2024-01-05 03:10_

---

_Closed by @zanieb on 2024-01-05 03:10_

---

_Branch deleted on 2024-01-05 03:10_

---
