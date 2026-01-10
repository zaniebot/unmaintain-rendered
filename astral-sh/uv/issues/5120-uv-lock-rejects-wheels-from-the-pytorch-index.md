```yaml
number: 5120
title: "`uv lock` rejects wheels from the PyTorch index"
type: issue
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
created_at: 2024-07-16T18:19:46Z
updated_at: 2024-07-17T20:29:50Z
url: https://github.com/astral-sh/uv/issues/5120
synced_at: 2026-01-10T05:31:37Z
```

# `uv lock` rejects wheels from the PyTorch index

---

_Issue opened by @charliermarsh on 2024-07-16 18:19_

These don't have hashes, so the lockfile rejects them. We either need to loosen that constraint, or compute hashes for all wheels from indexes that lack them.

---

_Label `preview` added by @charliermarsh on 2024-07-16 18:19_

---

_Comment by @BurntSushi on 2024-07-16 18:21_

Ref #4924

---

_Comment by @dimbleby on 2024-07-16 19:49_

per the last few comments at https://github.com/pytorch/pytorch/issues/76557: pytorch indexes mostly do have hashes these days.

While automating the process has apparently had its hiccups - they seem willing to engage with requests to fix cases where the hashes are missing.

---

_Comment by @charliermarsh on 2024-07-16 19:54_

Oh nice! (We do still need this in general since not all registries will have hashes, but that's really helpful.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 02:36_

---

_Closed by @charliermarsh on 2024-07-17 20:29_

---

_Closed by @charliermarsh on 2024-07-17 20:29_

---
