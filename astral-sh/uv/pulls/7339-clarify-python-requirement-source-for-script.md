```yaml
number: 7339
title: Clarify Python requirement source for script incompatibilities
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/script-err
created_at: 2024-09-12T19:06:45Z
updated_at: 2024-09-12T19:19:43Z
url: https://github.com/astral-sh/uv/pull/7339
synced_at: 2026-01-12T16:07:47Z
```

# Clarify Python requirement source for script incompatibilities

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/7293.


---

_Marked ready for review by @charliermarsh on 2024-09-12 19:06_

---

_Label `error messages` added by @charliermarsh on 2024-09-12 19:06_

---

_@zanieb reviewed on 2024-09-12 19:10_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:421 on 2024-09-12 19:10_

```suggestion
    warning: The Python request from `.python-version` resolved to Python 3.8.[X], which is incompatible with the script's Python requirement: `>=3.11`
```

---

_@zanieb approved on 2024-09-12 19:11_

---

_@charliermarsh reviewed on 2024-09-12 19:12_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:421 on 2024-09-12 19:12_

Good catch.

---

_Merged by @charliermarsh on 2024-09-12 19:19_

---

_Closed by @charliermarsh on 2024-09-12 19:19_

---

_Branch deleted on 2024-09-12 19:19_

---
