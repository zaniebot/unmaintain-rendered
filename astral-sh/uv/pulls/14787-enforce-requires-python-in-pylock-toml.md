```yaml
number: 14787
title: "Enforce `requires-python` in `pylock.toml`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pep-751-requires-python
created_at: 2025-07-21T12:56:27Z
updated_at: 2025-07-21T14:37:15Z
url: https://github.com/astral-sh/uv/pull/14787
synced_at: 2026-01-12T16:11:25Z
```

# Enforce `requires-python` in `pylock.toml`

---

_@charliermarsh_

## Summary

Turns out we weren't validating this at install-time.


---

_Review requested from @zanieb by @charliermarsh on 2025-07-21 12:56_

---

_Review requested from @konstin by @charliermarsh on 2025-07-21 12:56_

---

_Label `bug` added by @charliermarsh on 2025-07-21 12:56_

---

_Marked ready for review by @charliermarsh on 2025-07-21 12:59_

---

_@zanieb reviewed on 2025-07-21 13:41_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11617 on 2025-07-21 13:41_

nit: I'd probably just assert success here since the output isn't relevant and it's hard to tell if the warning is part of the test case

---

_@zanieb reviewed on 2025-07-21 13:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11616 on 2025-07-21 13:42_

nit: Why "managed"?

---

_@zanieb approved on 2025-07-21 13:42_

---

_Merged by @charliermarsh on 2025-07-21 14:37_

---

_Closed by @charliermarsh on 2025-07-21 14:37_

---

_Branch deleted on 2025-07-21 14:37_

---
