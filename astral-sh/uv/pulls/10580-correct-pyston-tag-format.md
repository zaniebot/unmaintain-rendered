```yaml
number: 10580
title: Correct Pyston tag format
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/all-tags
created_at: 2025-01-14T01:46:58Z
updated_at: 2025-01-14T02:31:12Z
url: https://github.com/astral-sh/uv/pull/10580
synced_at: 2026-01-10T11:44:58Z
```

# Correct Pyston tag format

---

_Pull request opened by @charliermarsh on 2025-01-14 01:46_

## Summary

Empirically, it looks like the format here is slightly different than what we had in the code? Our integration test caught it.

---

_Label `bug` added by @charliermarsh on 2025-01-14 01:53_

---

_@zanieb approved on 2025-01-14 01:54_

Yay integration tests — I presume you checked this matches a spec?

---

_Comment by @charliermarsh on 2025-01-14 02:02_

Trying... I don't know how the ABI tag is created, but the language tag is meant to use `sys.implementation.name` (https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/), which returns `pyston`, I think? https://github.com/pyston/pyston/blob/fe8afded6166e0774a62bac627e3fd3ba92a41d2/configure.ac#L4796

I have no idea where the ABI tag comes from. It looks like the wheel here is `numpy-1.24.4-pyston38-pyston_23_x86_64_linux_gnu-linux_x86_64.whl`, so the ABI tag is... `pyston_23_x86_64_linux_gnu`? Huh? Why did this even work before?

---

_Merged by @charliermarsh on 2025-01-14 02:31_

---

_Closed by @charliermarsh on 2025-01-14 02:31_

---

_Branch deleted on 2025-01-14 02:31_

---
