```yaml
number: 2256
title: Use released packse for scenario updates
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/scenarios
created_at: 2024-03-07T00:57:20Z
updated_at: 2024-03-07T17:40:55Z
url: https://github.com/astral-sh/uv/pull/2256
synced_at: 2026-01-12T16:04:56Z
```

# Use released packse for scenario updates

---

_@zanieb_

- Now that `packse` is being published to PyPI we can install it from there.
- Tweaks the tooling around scenario updates to manage a temporary virtual environment for you.
- Makes use of a new index URL
- Includes local version segment scenarios (supersedes https://github.com/astral-sh/uv/pull/2022)

---

_Label `testing` added by @zanieb on 2024-03-07 00:57_

---

_@zanieb reviewed on 2024-03-07 01:03_

---

_Review comment by @zanieb on `scripts/scenarios/generate.py`:1 on 2024-03-07 01:03_

This is a shortened replacement for `update.py`, mostly unchanged.

---

_@zanieb reviewed on 2024-03-07 01:16_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile_scenarios.rs`:30 on 2024-03-07 01:16_

This is a major change — no more reliance on PyPI itself (which also means we can be spec-incompliant)

---

_@zanieb reviewed on 2024-03-07 01:17_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile_scenarios.rs`:78 on 2024-03-07 01:17_

I'll simplify the filters in a follow-up, this is in an attempt to minimize the diff

---

_Marked ready for review by @zanieb on 2024-03-07 01:28_

---

_@zanieb reviewed on 2024-03-07 04:20_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile_scenarios.rs`:30 on 2024-03-07 04:20_

We also have the capability to compile the static index once at the start of a test run. I'd want to do some more work to make sure that's fast and such but it could be more convenient than requiring a packse release to add a new scenario.

---

_@charliermarsh reviewed on 2024-03-07 04:39_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile_scenarios.rs`:30 on 2024-03-07 04:39_

Wow, nice.

---

_@charliermarsh approved on 2024-03-07 04:39_

---

_@konstin approved on 2024-03-07 10:01_

---

_Merged by @zanieb on 2024-03-07 17:40_

---

_Closed by @zanieb on 2024-03-07 17:40_

---

_Branch deleted on 2024-03-07 17:40_

---
