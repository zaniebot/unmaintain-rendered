```yaml
number: 996
title: Add nextest config for retry of flaky test
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/flake
created_at: 2024-01-19T02:31:35Z
updated_at: 2024-01-19T05:53:22Z
url: https://github.com/astral-sh/uv/pull/996
synced_at: 2026-01-12T16:04:21Z
```

# Add nextest config for retry of flaky test

---

_@zanieb_

We should definitely sort this out per #863 but until then I want to stop getting false reports of failed CI please and this seems better than turning off the test entirely.

Alternatively #993

---

_Label `internal` added by @zanieb on 2024-01-19 02:32_

---

_Review comment by @zanieb on `.config/nextest.toml`:8 on 2024-01-19 02:33_

We could pick a higher number

---

_@zanieb reviewed on 2024-01-19 02:33_

---

_Comment by @zanieb on 2024-01-19 02:35_

I added the test that flaked in https://github.com/astral-sh/puffin/actions/runs/7578635628/job/20641539227 too

This might not be a single test but non-determinism in these ranges in multiple cases -.-

---

_Label `testing` added by @zanieb on 2024-01-19 02:41_

---

_@charliermarsh reviewed on 2024-01-19 05:27_

---

_Review comment by @charliermarsh on `.config/nextest.toml`:8 on 2024-01-19 05:27_

Yeah, I vote we increase it above two.

---

_@charliermarsh reviewed on 2024-01-19 05:28_

---

_Review comment by @charliermarsh on `.config/nextest.toml`:8 on 2024-01-19 05:28_

I swear I hit this like 20-30% of the time.

---

_@zanieb reviewed on 2024-01-19 05:38_

---

_Review comment by @zanieb on `.config/nextest.toml`:8 on 2024-01-19 05:38_

```suggestion
retries = 3
```

---

_Merged by @charliermarsh on 2024-01-19 05:53_

---

_Closed by @charliermarsh on 2024-01-19 05:53_

---

_Branch deleted on 2024-01-19 05:53_

---

_Comment by @charliermarsh on 2024-01-19 05:53_

Merging to save myself, thank you!

---
