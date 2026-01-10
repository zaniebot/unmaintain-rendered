---
number: 5661
title: Fork priorization should create a converging transformers resolution
type: issue
state: closed
author: konstin
labels:
  - resolver
assignees: []
created_at: 2024-07-31T15:48:06Z
updated_at: 2025-01-14T18:50:24Z
url: https://github.com/astral-sh/uv/issues/5661
synced_at: 2026-01-10T01:23:51Z
---

# Fork priorization should create a converging transformers resolution

---

_Issue opened by @konstin on 2024-07-31 15:48_

When resolving transformers (#5657) in universal mode, we resolve datasets v2.14.4, v2.20.0 and fsspec v2024.5.0, v2024.6.1 while datasets v2.20.0 and fsspec v2024.5.0 would work for both branches in a single converging resolution. We should figure out why that happens and if we can avoid it with smarter fork prioritization (or if it unavoidable with reasonable heuristics and we need to rely on the fork markers serialization for stability in this case).

---

_Label `resolver` added by @konstin on 2024-07-31 15:48_

---

_Comment by @charliermarsh on 2024-07-31 15:48_

👍 The only thing I noticed here is that we select that datasets version prior to forking.

---

_Comment by @konstin on 2025-01-14 18:50_

This got fixed in an unrelated PR.

---

_Closed by @konstin on 2025-01-14 18:50_

---
