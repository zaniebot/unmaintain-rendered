```yaml
number: 11086
title: Use a mirror with upload timestamps for PyTorch
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/mirror
created_at: 2025-01-30T00:37:42Z
updated_at: 2025-01-30T13:40:38Z
url: https://github.com/astral-sh/uv/pull/11086
synced_at: 2026-01-10T11:10:34Z
```

# Use a mirror with upload timestamps for PyTorch

---

_Pull request opened by @charliermarsh on 2025-01-30 00:37_

## Summary

This PR migrates all of our PyTorch tests to use our own mirror, which includes upload timestamps that we can use to enforce `--excludes-newer`, making the tests far more stable over time. (Today, if you checkout old versions of `uv`, many of the PyTorch tests will fail, since the index contents drift over time.)

Some snapshots changed in this PR (see, e.g., `universal_nested_overlapping_local_requirement`). The underlying reason is that I used the current timestamp when setting upload times in the PyTorch mirror, but those tests read from both the PyTorch `--find-links` index _and_ PyPI. I guess we don't omit `--find-links` entries based on `--excludes-newer`? That might be a bug. But I had to _increase_ the `--excludes-newer` to include the PyTorch mirror's `--find-links`, which meant pulling in some newer packages from PyPI too. This is fine: it's a one-time churn, and they'll be stable going forward.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-30 00:40_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 00:40_

---

_Review requested from @konstin by @charliermarsh on 2025-01-30 00:40_

---

_Label `testing` added by @charliermarsh on 2025-01-30 00:40_

---

_@zanieb approved on 2025-01-30 01:00_

---

_Merged by @charliermarsh on 2025-01-30 01:02_

---

_Closed by @charliermarsh on 2025-01-30 01:02_

---

_Branch deleted on 2025-01-30 01:02_

---

_@konstin reviewed on 2025-01-30 10:49_

:100:

---

_@BurntSushi reviewed on 2025-01-30 13:40_

w00t!

---
