```yaml
number: 7691
title: Treat the base Conda environment as a system environment
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: tracking/050
head: zb/conda-base
created_at: 2024-09-25T19:00:20Z
updated_at: 2024-10-21T22:23:51Z
url: https://github.com/astral-sh/uv/pull/7691
synced_at: 2026-01-12T16:07:57Z
```

# Treat the base Conda environment as a system environment

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7124
Closes https://github.com/astral-sh/uv/issues/7137

---

_Label `breaking` added by @zanieb on 2024-09-25 19:00_

---

_Marked ready for review by @zanieb on 2024-09-25 20:58_

---

_@charliermarsh reviewed on 2024-09-25 21:39_

---

_Review comment by @charliermarsh on `crates/uv-python/src/virtualenv.rs`:63 on 2024-09-25 21:39_

I might suggest making this an enum rather than a `bool` so that it's more self-documenting.

---

_@charliermarsh reviewed on 2024-09-25 21:40_

---

_Review comment by @charliermarsh on `crates/uv-python/src/virtualenv.rs`:94 on 2024-09-25 21:40_

You can probably do `name.to_str().and_then(...)`, since if it's not UTF-8, it can't match `base` or `root`.

---

_@charliermarsh approved on 2024-09-25 21:40_

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-01 19:50_

---

_Merged by @zanieb on 2024-10-21 22:23_

---

_Closed by @zanieb on 2024-10-21 22:23_

---

_Branch deleted on 2024-10-21 22:23_

---
