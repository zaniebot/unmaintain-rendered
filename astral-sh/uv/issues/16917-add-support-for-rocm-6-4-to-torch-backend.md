---
number: 16917
title: Add support for ROCm 6.4 to --torch-backend
type: issue
state: closed
author: nsburbank
labels:
  - enhancement
assignees: []
created_at: 2025-12-01T21:49:30Z
updated_at: 2025-12-02T01:27:22Z
url: https://github.com/astral-sh/uv/issues/16917
synced_at: 2026-01-10T01:26:11Z
---

# Add support for ROCm 6.4 to --torch-backend

---

_Issue opened by @nsburbank on 2025-12-01 21:49_

### Summary

The stable PyTorch builds at pytorch,org have included builds for ROCm 6.4 since PyTorch 2.8. It would be great to add this to the --torch-backend logic, similar to what was done for cu130 in https://github.com/astral-sh/uv/pull/16321.

I'm not sufficiently expert to generate a PR myself, which is why I'm filing this as an issue.

### Example

Makes the uv pip install torch --torch-backend=auto more useful by adding awareness of recent stable builds for AMD GPUs

---

_Label `enhancement` added by @nsburbank on 2025-12-01 21:49_

---

_Comment by @charliermarsh on 2025-12-01 21:50_

Good call, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-01 23:43_

---

_Referenced in [astral-sh/uv#16919](../../astral-sh/uv/pulls/16919.md) on 2025-12-01 23:43_

---

_Closed by @charliermarsh on 2025-12-02 01:27_

---
