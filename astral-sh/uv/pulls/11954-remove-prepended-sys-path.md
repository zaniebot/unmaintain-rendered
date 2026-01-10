```yaml
number: 11954
title: Remove prepended sys.path
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/remove-prepended-sys-path
created_at: 2025-03-04T15:40:22Z
updated_at: 2025-03-04T16:00:27Z
url: https://github.com/astral-sh/uv/pull/11954
synced_at: 2026-01-10T11:10:39Z
```

# Remove prepended sys.path

---

_Pull request opened by @konstin on 2025-03-04 15:40_

We prepend the interpreter discovery in a temporary path to `sys.path`, which we have to strip to avoid the `sys.path` value containing a then-deleted temp dir.

---

_Label `bug` added by @konstin on 2025-03-04 15:40_

---

_Comment by @charliermarsh on 2025-03-04 15:41_

Can you explain this in a bit more detail? What is the temporary path? How does it get added? I thought we ran with `-I`.

---

_Comment by @konstin on 2025-03-04 15:47_

It happens here:

https://github.com/astral-sh/uv/blob/f44aba0a968a2bfa84a9532ce0c0e78e4ccf0ab2/crates/uv-python/src/interpreter.rs#L744-L747

---

_Comment by @charliermarsh on 2025-03-04 15:48_

Great, thanks! Can you expand the comment to explain (in `get_interpreter_info.py`) that this is the path containing the script itself, and we add it so that it can be imported, etc.?

---

_@charliermarsh approved on 2025-03-04 15:49_

---

_Merged by @konstin on 2025-03-04 16:00_

---

_Closed by @konstin on 2025-03-04 16:00_

---

_Branch deleted on 2025-03-04 16:00_

---
