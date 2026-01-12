```yaml
number: 15449
title: Avoid introducing unnecessary system dependency on CUDA
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dep
created_at: 2025-08-22T10:38:19Z
updated_at: 2025-08-22T11:06:25Z
url: https://github.com/astral-sh/uv/pull/15449
synced_at: 2026-01-12T16:11:44Z
```

# Avoid introducing unnecessary system dependency on CUDA

---

_@charliermarsh_

## Summary

Packages like `triton` should come from the PyTorch index, but they don't actually vary across (e.g.) the `cu128` or `cu129` indexes.

Closes https://github.com/astral-sh/uv/issues/15446.

## Test Plan

Validate that the following pins to `cu128`, rather than `cpu`:

```
echo "vllm\ntorch==2.7.1+cu128" | cargo run pip compile --torch-backend=auto --extra-index-url https://wheels.vllm.ai/b2f6c247a9b84556a8ea0e75bb4a2db765ff3315 - --python-platform linux --python-version 3.13 -v
```


---

_Label `bug` added by @charliermarsh on 2025-08-22 10:38_

---

_Review requested from @konstin by @charliermarsh on 2025-08-22 10:38_

---

_Marked ready for review by @charliermarsh on 2025-08-22 10:40_

---

_@konstin approved on 2025-08-22 11:01_

---

_Merged by @charliermarsh on 2025-08-22 11:06_

---

_Closed by @charliermarsh on 2025-08-22 11:06_

---

_Branch deleted on 2025-08-22 11:06_

---
