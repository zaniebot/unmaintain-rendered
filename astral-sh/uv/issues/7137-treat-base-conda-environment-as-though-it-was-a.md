```yaml
number: 7137
title: Treat base conda environment as though it was a system Python
type: issue
state: closed
author: notatallshaw
labels:
  - needs-decision
  - breaking
assignees: []
created_at: 2024-09-06T21:23:04Z
updated_at: 2024-11-07T20:29:57Z
url: https://github.com/astral-sh/uv/issues/7137
synced_at: 2026-01-12T15:59:10Z
```

# Treat base conda environment as though it was a system Python

---

_@notatallshaw_

The base environment of conda is often not something that should be installed into as it can break conda.

It appears you can detect if the base environment is currently activated if `CONDA_PREFIX_1` is not set and/or `CONDA_DEFAULT_ENV` is equal to `base` (on very old versions of conda this was named `root`) : https://conda.io/projects/conda/en/latest/dev-guide/deep-dives/activation.html#conda-activate

Perhaps worth requiring users to use `--system` to install into this base conda environment?

---

_Label `needs-decision` added by @zanieb on 2024-09-07 15:17_

---

_Label `breaking` added by @zanieb on 2024-09-07 15:17_

---

_Closed by @zanieb on 2024-11-07 20:29_

---
