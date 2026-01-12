```yaml
number: 5462
title: "Use `sitecustomize.py` to implement environment layering"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-07-25T23:19:13Z
updated_at: 2024-07-25T23:32:46Z
url: https://github.com/astral-sh/uv/pull/5462
synced_at: 2026-01-12T16:06:49Z
```

# Use `sitecustomize.py` to implement environment layering

---

_@charliermarsh_

## Summary

After consultation with @carljm, we learned that modifying `PYTHONPATH` is insufficient, because Python won't resolve `.pth` files (editables) in the base environment. We also saw in https://github.com/astral-sh/uv/issues/5459 that continuously appending to `PYTHONPATH` can have some unintended effects.

This PR instead uses a `sitecustomize.py` in the ephemeral environment to add the base environment's `site-packages`.

Closes https://github.com/astral-sh/uv/issues/5459.


---

_Label `bug` added by @charliermarsh on 2024-07-25 23:19_

---

_Marked ready for review by @charliermarsh on 2024-07-25 23:19_

---

_Label `preview` added by @charliermarsh on 2024-07-25 23:19_

---

_@charliermarsh reviewed on 2024-07-25 23:21_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:817 on 2024-07-25 23:21_

Amazingly, this test fails on main. Can't believe we hadn't tried this yet.

---

_Merged by @charliermarsh on 2024-07-25 23:32_

---

_Closed by @charliermarsh on 2024-07-25 23:32_

---

_Branch deleted on 2024-07-25 23:32_

---
