```yaml
number: 2662
title: Fall back to PEP 517 hooks for non-compliant PEP 621 metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/hatch
created_at: 2024-03-26T00:03:42Z
updated_at: 2024-03-26T02:28:40Z
url: https://github.com/astral-sh/uv/pull/2662
synced_at: 2026-01-12T16:05:09Z
```

# Fall back to PEP 517 hooks for non-compliant PEP 621 metadata

---

_@charliermarsh_

If you pass a `pyproject.toml` that use Hatch's context formatting API, we currently fail because the dependencies aren't valid under PEP 508. This PR makes the static metadata parsing a little more relaxed, so that we appropriately fall back to PEP 517 there.


---

_Label `compatibility` added by @charliermarsh on 2024-03-26 00:03_

---

_Marked ready for review by @charliermarsh on 2024-03-26 00:03_

---

_Merged by @charliermarsh on 2024-03-26 02:28_

---

_Closed by @charliermarsh on 2024-03-26 02:28_

---

_Branch deleted on 2024-03-26 02:28_

---
