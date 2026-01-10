---
number: 16319
title: torch-backend needs to be updated to support cu130
type: issue
state: closed
author: rajesh-s
labels: []
assignees: []
created_at: 2025-10-15T17:32:10Z
updated_at: 2025-10-15T19:10:09Z
url: https://github.com/astral-sh/uv/issues/16319
synced_at: 2026-01-10T01:26:05Z
---

# torch-backend needs to be updated to support cu130

---

_Issue opened by @rajesh-s on 2025-10-15 17:32_

https://github.com/astral-sh/uv/blob/83635a6c4509ac2730f5a3bd80e4a75ce52eca31/crates/uv-torch/src/backend.rs#L59

PyTorch just rolled out with CUDA 13.0 support. The following backend command is currently not supported with `uv`

`uv pip install torch torchvision --index-url https://download.pytorch.org/whl/cu130 --torch-backend=cu130`

---

_Comment by @charliermarsh on 2025-10-15 17:33_

Oh thanks, we'll add it today.

---

_Renamed from "torch-backend needs to be update to support cu130" to "torch-backend needs to be updated to support cu130" by @rajesh-s on 2025-10-15 17:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-10-15 17:35_

---

_Referenced in [astral-sh/uv#16321](../../astral-sh/uv/pulls/16321.md) on 2025-10-15 18:09_

---

_Closed by @charliermarsh on 2025-10-15 19:10_

---
