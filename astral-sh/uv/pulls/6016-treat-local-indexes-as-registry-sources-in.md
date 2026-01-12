```yaml
number: 6016
title: Treat local indexes as registry sources in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/find-links
created_at: 2024-08-12T01:12:51Z
updated_at: 2024-08-12T02:02:41Z
url: https://github.com/astral-sh/uv/pull/6016
synced_at: 2026-01-12T16:07:09Z
```

# Treat local indexes as registry sources in lockfile

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/6013.

Closes https://github.com/astral-sh/uv/issues/6014.

Adds test coverage for https://github.com/astral-sh/uv/issues/6015.


---

_Label `bug` added by @charliermarsh on 2024-08-12 01:12_

---

_Label `preview` added by @charliermarsh on 2024-08-12 01:12_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:6102 on 2024-08-12 01:13_

This strikes me as wrong: https://github.com/astral-sh/uv/issues/6015. These don't come from the linked registry.

---

_@charliermarsh reviewed on 2024-08-12 01:13_

---

_@charliermarsh reviewed on 2024-08-12 01:17_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:6102 on 2024-08-12 01:17_

We do kind of "allow" this today though. I think it's actually consistent with our semantic treatment of `--find-links`.

---

_Merged by @charliermarsh on 2024-08-12 02:02_

---

_Closed by @charliermarsh on 2024-08-12 02:02_

---

_Branch deleted on 2024-08-12 02:02_

---
