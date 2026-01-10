```yaml
number: 6994
title: "Reflect exit code in `uv tool run` and `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/code
created_at: 2024-09-04T03:29:10Z
updated_at: 2024-09-04T13:33:29Z
url: https://github.com/astral-sh/uv/pull/6994
synced_at: 2026-01-10T12:53:37Z
```

# Reflect exit code in `uv tool run` and `uv run`

---

_Pull request opened by @charliermarsh on 2024-09-04 03:29_

## Summary

Closes https://github.com/astral-sh/uv/issues/6710.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-04 03:29_

---

_Review requested from @konstin by @charliermarsh on 2024-09-04 03:29_

---

_Label `bug` added by @charliermarsh on 2024-09-04 03:29_

---

_@charliermarsh reviewed on 2024-09-04 03:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:237 on 2024-09-04 03:30_

Apparently on Unix you get `None` if it exits with a signal.

---

_@charliermarsh reviewed on 2024-09-04 03:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:226 on 2024-09-04 03:30_

I don't really understand when this could fail.

---

_Comment by @charliermarsh on 2024-09-04 03:47_

An alternative is that we use `exec`-style execution instead. Then we'd get exit codes, signals, etc. by default. The downside is that we can't do any of our own custom error-handling. So we'd lose these really nice error messages:

![Screenshot 2024-09-03 at 11 43 49 PM](https://github.com/user-attachments/assets/424030df-5b13-43a6-ad19-76b99e4f236e)


---

_@konstin approved on 2024-09-04 10:15_

---

_@zanieb reviewed on 2024-09-04 13:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:226 on 2024-09-04 13:10_

For some reason, I wrote this previously https://github.com/astral-sh/uv/blob/fd408c4ffa7d61948eb4d1a4ee2805c141730378/crates/uv/src/bin/uvx.rs#L50-L55

---

_@zanieb approved on 2024-09-04 13:11_

---

_Merged by @charliermarsh on 2024-09-04 13:33_

---

_Closed by @charliermarsh on 2024-09-04 13:33_

---

_Branch deleted on 2024-09-04 13:33_

---
