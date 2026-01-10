```yaml
number: 9905
title: "Patch additional `sysconfig` values such as AR at install time"
type: pull_request
state: merged
author: samypr100
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: patch-ar-and-friends
created_at: 2024-12-15T06:21:05Z
updated_at: 2024-12-15T15:30:16Z
url: https://github.com/astral-sh/uv/pull/9905
synced_at: 2026-01-10T12:00:01Z
```

# Patch additional `sysconfig` values such as AR at install time

---

_Pull request opened by @samypr100 on 2024-12-15 06:21_

## Summary

Minor follow up to https://github.com/astral-sh/uv/pull/9857 to patch AR.

Implements the AR replacement used in [sysconfigpatcher](https://github.com/bluss/sysconfigpatcher/blob/main/src/sysconfigpatcher.py#L54), namely

```python
DEFAULT_VARIABLE_UPDATES = {
    ...
    "AR": "ar",
}
```

## Test Plan

Added an additional test. Tested local python installs.

Related traces
```
TRACE Updated `AR` from `/tools/clang-linux64/bin/llvm-ar` to `ar`
```

---

_Marked ready for review by @samypr100 on 2024-12-15 06:32_

---

_Comment by @charliermarsh on 2024-12-15 13:43_

Awesome, thank you! I think I want to hold off on rewriting clang just yet... I'd rather make that opt-in or perhaps opt-out. Can we pare this down to just AR for now?

---

_Label `bug` added by @charliermarsh on 2024-12-15 13:43_

---

_Label `compatibility` added by @charliermarsh on 2024-12-15 13:43_

---

_Comment by @charliermarsh on 2024-12-15 14:45_

If you're up for it, you could also open the `clang` to `cc` change in a separate PR and we can discuss it there? But the `AR` change can merge immediately :)

---

_@charliermarsh approved on 2024-12-15 14:46_

---

_Comment by @samypr100 on 2024-12-15 14:50_

> If you're up for it, you could also open the `clang` to `cc` change in a separate PR and we can discuss it there? But the `AR` change can merge immediately :)

Yup, I was thinking the same thing 👍 

---

_Merged by @charliermarsh on 2024-12-15 15:27_

---

_Closed by @charliermarsh on 2024-12-15 15:27_

---

_Branch deleted on 2024-12-15 15:30_

---
