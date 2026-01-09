---
number: 11307
title: PD011 also applies to pytorch tensors
type: issue
state: closed
author: Modexus
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-05-06T13:25:32Z
updated_at: 2024-05-06T13:35:10Z
url: https://github.com/astral-sh/ruff/issues/11307
synced_at: 2026-01-07T13:12:15-06:00
---

# PD011 also applies to pytorch tensors

---

_Issue opened by @Modexus on 2024-05-06 13:25_

```python
torch.tensor([3, 2, 1]).sort().values
```

causes `PD011 ` even though it does not return a `numpy `value.

---

_Comment by @charliermarsh on 2024-05-06 13:35_

Thanks -- I think this is the same as #6432, so going to merge into that issue.

---

_Closed by @charliermarsh on 2024-05-06 13:35_

---

_Label `bug` added by @charliermarsh on 2024-05-06 13:35_

---

_Label `type-inference` added by @charliermarsh on 2024-05-06 13:35_

---
