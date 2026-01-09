---
number: 5012
title: Reject versions when there is no source dist and all wheels mismatch the python requirement
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-07-12T14:50:22Z
updated_at: 2024-07-16T01:01:39Z
url: https://github.com/astral-sh/uv/issues/5012
synced_at: 2026-01-07T13:12:17-06:00
---

# Reject versions when there is no source dist and all wheels mismatch the python requirement

---

_Issue opened by @konstin on 2024-07-12 14:50_

For [torch 1.10.0](https://pypi.org/project/torch/1.10.0/#files), there are wheel for python 3.6 - python 3.9, and there is no source dist. We should reject this version when resolving for python >= 3.10 since there is never any compatible wheel.

---

_Label `enhancement` added by @konstin on 2024-07-12 14:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 14:51_

---

_Comment by @charliermarsh on 2024-07-12 21:40_

This actually seems pretty hard now that we do our `requires-python` checks outside of the version map... Not sure how to do it.

---

_Referenced in [astral-sh/uv#5084](../../astral-sh/uv/pulls/5084.md) on 2024-07-16 00:48_

---

_Closed by @charliermarsh on 2024-07-16 01:01_

---

_Closed by @charliermarsh on 2024-07-16 01:01_

---
