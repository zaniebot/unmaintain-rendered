```yaml
number: 13688
title: Update packse scenarios
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/packse-up
created_at: 2025-05-27T19:42:22Z
updated_at: 2025-05-28T13:58:40Z
url: https://github.com/astral-sh/uv/pull/13688
synced_at: 2026-01-10T11:10:42Z
```

# Update packse scenarios

---

_Pull request opened by @zanieb on 2025-05-27 19:42_

Closes #13676 

See https://github.com/astral-sh/packse/pull/242 and https://github.com/astral-sh/packse/releases/tag/0.3.47

---

_@zanieb reviewed on 2025-05-27 19:44_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:69 on 2025-05-27 19:44_

Extra lines indicating min Python requirement would be resolved by https://github.com/astral-sh/packse/pull/243

---

_@zanieb reviewed on 2025-05-27 19:45_

---

_Review comment by @zanieb on `scripts/scenarios/templates/compile.mustache`:36 on 2025-05-27 19:45_

Looks like these were changed in the generated files during a refactor

---

_Review comment by @zanieb on `.python-versions`:10 on 2025-05-27 19:45_

Moving off of 3.8

---

_@zanieb reviewed on 2025-05-27 19:45_

---

_Marked ready for review by @zanieb on 2025-05-27 22:32_

---

_Assigned to @konstin by @zanieb on 2025-05-27 22:32_

---

_@zanieb reviewed on 2025-05-27 22:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:69 on 2025-05-27 22:48_

(which I will do _next_ because I'm not sure if it'll introduce more diff)

---

_Review requested from @konstin by @zanieb on 2025-05-28 13:18_

---

_@konstin approved on 2025-05-28 13:20_

---

_@konstin reviewed on 2025-05-28 13:20_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install_scenarios.rs`:69 on 2025-05-28 13:20_

It would reduce the diff a lot, but we need https://github.com/astral-sh/packse/pull/251 and follow-ups

---

_Label `internal` added by @konstin on 2025-05-28 13:20_

---

_@konstin reviewed on 2025-05-28 13:21_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install_scenarios.rs`:69 on 2025-05-28 13:21_

(I thought this would be an easy improvement and then stopped at https://github.com/astral-sh/packse/pull/251)

---

_Merged by @zanieb on 2025-05-28 13:58_

---

_Closed by @zanieb on 2025-05-28 13:58_

---

_Branch deleted on 2025-05-28 13:58_

---
