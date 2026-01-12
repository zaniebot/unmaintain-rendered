```yaml
number: 7303
title: "Avoid treating `.whl` sources as source distributions"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/path-source
created_at: 2024-09-11T18:58:55Z
updated_at: 2024-09-11T19:10:40Z
url: https://github.com/astral-sh/uv/pull/7303
synced_at: 2026-01-12T16:07:46Z
```

# Avoid treating `.whl` sources as source distributions

---

_@charliermarsh_

## Summary

The error messages here are incorrect.

Closes https://github.com/astral-sh/uv/issues/7284.


---

_@zanieb reviewed on 2024-09-11 19:05_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2248 on 2024-09-11 19:05_

The wheel can't be installed because it doesn't have a source distribution for the current platform?

---

_@zanieb reviewed on 2024-09-11 19:06_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2248 on 2024-09-11 19:06_

Is this message generally correct and you're using these for test cases?

Should we open an issue to add a special case for direct URLs to wheels or sdists?

---

_@zanieb reviewed on 2024-09-11 19:06_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2246 on 2024-09-11 19:06_

Kind of annoying we show a warning that matches the error message

---

_@charliermarsh reviewed on 2024-09-11 19:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:2246 on 2024-09-11 19:08_

This is a bug actually

---

_Merged by @charliermarsh on 2024-09-11 19:10_

---

_Closed by @charliermarsh on 2024-09-11 19:10_

---

_Branch deleted on 2024-09-11 19:10_

---

_@charliermarsh reviewed on 2024-09-11 19:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:2248 on 2024-09-11 19:10_

Yeah it's generally correct, you're right that it's off here though. I'll fix it up in a separate PR.

---
