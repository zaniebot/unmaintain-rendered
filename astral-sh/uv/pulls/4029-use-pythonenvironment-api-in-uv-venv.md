```yaml
number: 4029
title: "Use `PythonEnvironment` API in `uv venv`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/venv-python-env
created_at: 2024-06-04T23:30:12Z
updated_at: 2024-06-05T16:49:31Z
url: https://github.com/astral-sh/uv/pull/4029
synced_at: 2026-01-10T13:54:02Z
```

# Use `PythonEnvironment` API in `uv venv`

---

_Pull request opened by @zanieb on 2024-06-04 23:30_

There's no reason to be reaching into the lower-level `find_interpreter` manually here.

---

_Label `internal` added by @zanieb on 2024-06-04 23:30_

---

_Marked ready for review by @zanieb on 2024-06-04 23:34_

---

_@charliermarsh reviewed on 2024-06-04 23:37_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:124 on 2024-06-04 23:37_

Does `uv venv` from within a venv still work?

---

_@zanieb reviewed on 2024-06-04 23:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:124 on 2024-06-04 23:40_

I'll verify this — probably not. I think the `find_default_interpreter` branch is actually important here.

Thanks for flagging.

---

_Converted to draft by @zanieb on 2024-06-04 23:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:124 on 2024-06-04 23:42_

I wonder if we can add test coverage for that case too...

---

_@zanieb reviewed on 2024-06-04 23:42_

---

_@zanieb reviewed on 2024-06-05 12:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:124 on 2024-06-05 12:56_

So what I have here is actually equivalent to the current code. I think `uv venv` from an active virtual environment doesn't read the interpreter from the virtual environment; I tried to write a test case and it's not working on `main`. Should it? Maybe this regressed in #3266 and we missed it because we didn't have test coverage?

---

_@zanieb reviewed on 2024-06-05 13:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:124 on 2024-06-05 13:15_

Adding test coverage in https://github.com/astral-sh/uv/pull/4045

---

_Marked ready for review by @zanieb on 2024-06-05 16:12_

---

_@zanieb reviewed on 2024-06-05 16:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:124 on 2024-06-05 16:13_

Discussed in Discord, the behavior that Charlie mentioned was changed in #3266 since we skip interpreters in the PATH that are not virtual environments.

There is no behavior change in this pull request and I think the existing behavior is okay for now since we haven't gotten any reports about the change that was released in 0.2.0.

---

_@charliermarsh approved on 2024-06-05 16:47_

---

_Merged by @zanieb on 2024-06-05 16:49_

---

_Closed by @zanieb on 2024-06-05 16:49_

---

_Branch deleted on 2024-06-05 16:49_

---
