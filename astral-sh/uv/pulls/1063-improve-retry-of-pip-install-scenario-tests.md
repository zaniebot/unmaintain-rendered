```yaml
number: 1063
title: Improve retry of pip install scenario tests
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/retry
created_at: 2024-01-23T15:30:58Z
updated_at: 2024-01-23T16:35:17Z
url: https://github.com/astral-sh/uv/pull/1063
synced_at: 2026-01-12T16:04:23Z
```

# Improve retry of pip install scenario tests

---

_@zanieb_

Follow-up to #996 with retries of the full `pip_install_scenario` module since there are a few tests that flake and tracking down the non-determinism is proving to be quite complicated.

Adds a profile that can be used to disable retries, e.g. `--profile no-retry` when you expect the scenario to change.

As before, hopefully this is a short-term measure.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-23 15:32_

---

_@zanieb reviewed on 2024-01-23 15:32_

---

_Review comment by @zanieb on `.config/nextest.toml`:7 on 2024-01-23 15:32_

This syntax actually wasn't valid. Needs to be `test(...)|test(...)`

---

_Label `testing` added by @zanieb on 2024-01-23 15:35_

---

_Merged by @zanieb on 2024-01-23 16:35_

---

_Closed by @zanieb on 2024-01-23 16:35_

---

_Branch deleted on 2024-01-23 16:35_

---
