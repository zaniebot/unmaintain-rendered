```yaml
number: 935
title: "Bump to latest packse version with \"extras\" scenarios"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/packse-extras
created_at: 2024-01-15T19:46:19Z
updated_at: 2024-01-17T19:25:49Z
url: https://github.com/astral-sh/uv/pull/935
synced_at: 2026-01-12T16:04:18Z
```

# Bump to latest packse version with "extras" scenarios

---

_@zanieb_

Includes:

- https://github.com/zanieb/packse/pull/83 (replaces some of the post-processing here)
- https://github.com/zanieb/packse/pull/82
- https://github.com/zanieb/packse/pull/81

---

_Label `testing` added by @zanieb on 2024-01-16 14:27_

---

_Marked ready for review by @zanieb on 2024-01-16 14:27_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:449 on 2024-01-16 14:28_

We're using `packaging.requirement.Requirement` to parse these in packse now and it changed the order, which interestingly changes our error messages.

---

_@zanieb reviewed on 2024-01-16 14:28_

---

_@zanieb reviewed on 2024-01-16 14:29_

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:201 on 2024-01-16 14:29_

This post-processing moved into `packse inspect`

---

_Merged by @zanieb on 2024-01-17 19:25_

---

_Closed by @zanieb on 2024-01-17 19:25_

---

_Branch deleted on 2024-01-17 19:25_

---
