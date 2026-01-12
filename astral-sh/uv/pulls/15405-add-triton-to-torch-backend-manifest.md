```yaml
number: 15405
title: "Add `triton` to `torch-backend` manifest"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/triton
created_at: 2025-08-21T13:13:26Z
updated_at: 2025-08-21T13:23:13Z
url: https://github.com/astral-sh/uv/pull/15405
synced_at: 2026-01-12T16:11:43Z
```

# Add `triton` to `torch-backend` manifest

---

_@charliermarsh_

## Summary

The PyTorch team publishes ARM Linux wheels for `triton` to the PyTorch index, which aren't available on PyPI.

## Test Plan

```
echo "torch" | cargo run pip compile - --torch-backend=cu128 --python-platform aarch64-unknown-linux-gnu --python-version 3.13
```

Previously failed because it couldn't find a compatible `triton` wheel.

---

_Label `bug` added by @charliermarsh on 2025-08-21 13:13_

---

_Marked ready for review by @charliermarsh on 2025-08-21 13:13_

---

_Merged by @charliermarsh on 2025-08-21 13:23_

---

_Closed by @charliermarsh on 2025-08-21 13:23_

---

_Branch deleted on 2025-08-21 13:23_

---
