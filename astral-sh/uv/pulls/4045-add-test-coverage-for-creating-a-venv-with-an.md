```yaml
number: 4045
title: Add test coverage for creating a venv with an active venv
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/venv-from-venv
created_at: 2024-06-05T13:12:09Z
updated_at: 2024-07-02T14:57:21Z
url: https://github.com/astral-sh/uv/pull/4045
synced_at: 2026-01-12T16:06:00Z
```

# Add test coverage for creating a venv with an active venv

---

_@zanieb_

_No description provided._

---

_Label `testing` added by @zanieb on 2024-06-05 13:12_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:659 on 2024-06-05 13:12_

All these test outcomes are "wrong" given my understanding of the desired behavior.

---

_@zanieb reviewed on 2024-06-05 13:12_

---

_Comment by @zanieb on 2024-06-05 13:13_

Per https://github.com/astral-sh/uv/pull/3728 I think that we actually want the behaviors encoded in the snapshots (not the comments)...

---

_Comment by @zanieb on 2024-06-05 13:15_

I'm not sure if we _should_ be reading interpreters from the active environment. I can't find a place where we discussed that.

---

_Closed by @zanieb on 2024-07-02 14:57_

---
