```yaml
number: 12478
title: Support CUDA-specific indices beyond PyTorch
type: issue
state: open
author: heiner
labels:
  - enhancement
assignees: []
created_at: 2025-03-26T06:52:27Z
updated_at: 2025-04-01T19:02:23Z
url: https://github.com/astral-sh/uv/issues/12478
synced_at: 2026-01-12T16:01:04Z
```

# Support CUDA-specific indices beyond PyTorch

---

_@heiner_

### Summary

First of all, thanks for an amazing tool!

The new [Automatic backend selection](https://docs.astral.sh/uv/guides/integration/pytorch/#automatic-backend-selection) feature is great. However, in certain situations it would be good to extend this concept, e.g. for:

- Private PyPI repos with torch wheels,
- Other CUDA-dependent wheels, e.g. JAX (again, possibly in non-standard indices).

Ideally, something like PEP 508-style specifiers that allow selecting CUDA versions would be supported (this would potentially even allow using a single index, if CUDA versions are specified in the package version).

### Example

_No response_

---

_Label `enhancement` added by @heiner on 2025-03-26 06:52_

---

_Comment by @zanieb on 2025-04-01 19:02_

I think this is better captured by the larger https://wheelnext.dev effort

---
