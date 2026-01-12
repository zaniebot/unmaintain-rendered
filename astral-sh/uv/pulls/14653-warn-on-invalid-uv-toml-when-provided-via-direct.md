```yaml
number: 14653
title: "Warn on invalid `uv.toml` when provided via direct path"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2025-07-16T13:34:55Z
updated_at: 2025-07-16T13:48:17Z
url: https://github.com/astral-sh/uv/pull/14653
synced_at: 2026-01-12T16:11:19Z
```

# Warn on invalid `uv.toml` when provided via direct path

---

_@charliermarsh_

## Summary

We validate the `uv.toml` when it's discovered automatically, but not when provided via `--config-file`. The same limitations exist, though -- I think the lack of enforcement is just an oversight.

Closes https://github.com/astral-sh/uv/issues/14650.


---

_Label `error messages` added by @charliermarsh on 2025-07-16 13:34_

---

_Marked ready for review by @charliermarsh on 2025-07-16 13:34_

---

_@zanieb approved on 2025-07-16 13:36_

---

_Comment by @zanieb on 2025-07-16 13:36_

(A test case would be nice)

---

_Comment by @charliermarsh on 2025-07-16 13:37_

Yup done.

---

_Merged by @charliermarsh on 2025-07-16 13:48_

---

_Closed by @charliermarsh on 2025-07-16 13:48_

---

_Branch deleted on 2025-07-16 13:48_

---
