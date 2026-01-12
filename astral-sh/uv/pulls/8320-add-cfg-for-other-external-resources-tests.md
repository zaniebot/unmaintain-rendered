```yaml
number: 8320
title: Add cfg for other external resources tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cfg-external-tests
created_at: 2024-10-18T07:02:07Z
updated_at: 2024-10-18T16:30:16Z
url: https://github.com/astral-sh/uv/pull/8320
synced_at: 2026-01-12T16:08:15Z
```

# Add cfg for other external resources tests

---

_@konstin_

Fixes #8295

---

_Label `internal` added by @konstin on 2024-10-18 07:02_

---

_@zanieb approved on 2024-10-18 14:35_

---

_@charliermarsh reviewed on 2024-10-18 14:49_

---

_Review comment by @charliermarsh on `crates/uv/Cargo.toml`:122 on 2024-10-18 14:49_

I think this should be called `crates.io` or something, to match `pypi`. PyPI is also technically "external".

---

_@charliermarsh requested changes on 2024-10-18 14:49_

Lets make this a little more specific IMO.

---

_Comment by @konstin on 2024-10-18 15:20_

What's the use case for making them this specific?

---

_@zanieb reviewed on 2024-10-18 15:57_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:122 on 2024-10-18 15:57_

Agree, I was surprised by this when reading.

---

_@charliermarsh approved on 2024-10-18 16:30_

---

_Merged by @charliermarsh on 2024-10-18 16:30_

---

_Closed by @charliermarsh on 2024-10-18 16:30_

---

_Branch deleted on 2024-10-18 16:30_

---

_Comment by @charliermarsh on 2024-10-18 16:30_

Thank you!

---
