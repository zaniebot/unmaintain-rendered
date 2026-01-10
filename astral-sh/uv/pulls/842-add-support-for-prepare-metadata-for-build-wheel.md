```yaml
number: 842
title: "Add support for `prepare_metadata_for_build_wheel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: charlie/prepare-metadata
created_at: 2024-01-08T22:14:21Z
updated_at: 2024-01-10T00:07:38Z
url: https://github.com/astral-sh/uv/pull/842
synced_at: 2026-01-10T15:39:02Z
```

# Add support for `prepare_metadata_for_build_wheel`

---

_Pull request opened by @charliermarsh on 2024-01-08 22:14_

## Summary

This PR adds support for `prepare_metadata_for_build_wheel`, which allows us to determine source distribution metadata without building the source distribution. This represents an optimization for the resolver, as we can skip the expensive build phase for build backends that support it.

For reference, `prepare_metadata_for_build_wheel` seems to be supported by:

- `hatchling` (as of [1.0.9](https://hatch.pypa.io/latest/history/hatchling/#hatchling-v1.9.0)).
- `flit`
- `setuptools`

In fact, it seems to work for every backend _except_ those using legacy `setup.py`.

Closes #599.


---

_@charliermarsh reviewed on 2024-01-08 22:15_

---

_@charliermarsh reviewed on 2024-01-08 22:15_

---

_@charliermarsh reviewed on 2024-01-08 22:16_

---

_Review requested from @konstin by @charliermarsh on 2024-01-08 22:16_

---

_Label `enhancement` added by @charliermarsh on 2024-01-08 22:16_

---

_Label `performance` added by @charliermarsh on 2024-01-08 22:16_

---

_@konstin reviewed on 2024-01-08 22:24_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-08 22:45_

---

_@charliermarsh reviewed on 2024-01-08 22:47_

---

_@charliermarsh reviewed on 2024-01-09 00:13_

---

_@konstin approved on 2024-01-09 22:17_

Do we have a test case for both a backend with `prepare_metadata_for_build_wheel` and without? If the existing test cases cover this we should document it.

Otherwise only very minor comments :shipit: 

---

_@charliermarsh reviewed on 2024-01-09 23:20_

---

_Merged by @charliermarsh on 2024-01-10 00:07_

---

_Closed by @charliermarsh on 2024-01-10 00:07_

---

_Branch deleted on 2024-01-10 00:07_

---
