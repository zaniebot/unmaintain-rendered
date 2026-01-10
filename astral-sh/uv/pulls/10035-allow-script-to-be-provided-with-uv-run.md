```yaml
number: 10035
title: "Allow `--script` to be provided with `uv run -`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/script-stdin
created_at: 2024-12-19T17:14:06Z
updated_at: 2024-12-19T17:52:59Z
url: https://github.com/astral-sh/uv/pull/10035
synced_at: 2026-01-10T12:00:01Z
```

# Allow `--script` to be provided with `uv run -`

---

_Pull request opened by @charliermarsh on 2024-12-19 17:14_

## Summary

Closes #10021.


---

_Label `bug` added by @charliermarsh on 2024-12-19 17:14_

---

_@zanieb reviewed on 2024-12-19 17:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:2994 on 2024-12-19 17:51_

`--gui-script` should just "work" on Unix too — so you can use a script on both platforms.

---

_@zanieb reviewed on 2024-12-19 17:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:2994 on 2024-12-19 17:51_

In other words, if you're not going to assert which executable is used, we should combine the tests.

---

_Merged by @charliermarsh on 2024-12-19 17:52_

---

_Closed by @charliermarsh on 2024-12-19 17:52_

---

_Branch deleted on 2024-12-19 17:52_

---
