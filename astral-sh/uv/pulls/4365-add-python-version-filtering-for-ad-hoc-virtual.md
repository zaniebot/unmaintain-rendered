```yaml
number: 4365
title: Add Python version filtering for ad-hoc virtual environments
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/filter-create-venv
created_at: 2024-06-17T18:09:15Z
updated_at: 2024-06-17T20:15:01Z
url: https://github.com/astral-sh/uv/pull/4365
synced_at: 2026-01-12T16:06:11Z
```

# Add Python version filtering for ad-hoc virtual environments

---

_@zanieb_

Extends #4364 automatically adding filters to the test context for additional test virtual environments.

It turns out that the `pip sync` tests were really on the loose with their virtual environment creation and it was difficult to use the new helpers because they require mutability and the tests had immutable borrows to extend the filters 😭. I refactored the tests to just reset the environment.

---

_Label `testing` added by @zanieb on 2024-06-17 18:09_

---

_Marked ready for review by @zanieb on 2024-06-17 19:17_

---

_Review requested from @ibraheemdev by @zanieb on 2024-06-17 19:22_

---

_@zanieb reviewed on 2024-06-17 19:28_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:518 on 2024-06-17 19:28_

I moved these here then noticed we don't actually need them in tests after adding `reset` so I made them private.

---

_Merged by @zanieb on 2024-06-17 20:15_

---

_Closed by @zanieb on 2024-06-17 20:15_

---

_Branch deleted on 2024-06-17 20:15_

---
