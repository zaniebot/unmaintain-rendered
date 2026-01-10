```yaml
number: 4746
title: Replace tool environments on updated Python request
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/tool-env
created_at: 2024-07-02T22:31:34Z
updated_at: 2024-07-02T23:07:56Z
url: https://github.com/astral-sh/uv/pull/4746
synced_at: 2026-01-10T13:48:28Z
```

# Replace tool environments on updated Python request

---

_Pull request opened by @charliermarsh on 2024-07-02 22:31_

## Summary

Closes https://github.com/astral-sh/uv/issues/4741.


---

_Label `enhancement` added by @charliermarsh on 2024-07-02 22:31_

---

_Label `preview` added by @charliermarsh on 2024-07-02 22:31_

---

_@charliermarsh reviewed on 2024-07-02 22:33_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:161 on 2024-07-02 22:33_

Debatable but more robust?

---

_@charliermarsh reviewed on 2024-07-02 22:33_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:62 on 2024-07-02 22:33_

This is somewhat annoying.

---

_@charliermarsh reviewed on 2024-07-02 22:34_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:62 on 2024-07-02 22:34_

(Not a behavior change, just forced to explain why now. I'm not sure how to avoid it. I can try to make it lazy.)

---

_@zanieb reviewed on 2024-07-02 22:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:34_

Oh.. hm.

---

_@zanieb reviewed on 2024-07-02 22:35_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:1376 on 2024-07-02 22:35_

Can we log that the existing environment does not meet the Python version request?

---

_@charliermarsh reviewed on 2024-07-02 22:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:1376 on 2024-07-02 22:35_

Sure.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:36_

Yeah... we've talked before about writing relative paths in this specific case (with opt-in) to make scripts relocatable.

---

_@charliermarsh reviewed on 2024-07-02 22:36_

---

_@zanieb reviewed on 2024-07-02 22:37_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:161 on 2024-07-02 22:37_

Might be overkill but seems fine

---

_@zanieb reviewed on 2024-07-02 22:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:38_

We can also have a special entry point that calls into `uv` to determine what to do (I've kind of wanted this for other reasons)

---

_@zanieb approved on 2024-07-02 22:38_

---

_@charliermarsh reviewed on 2024-07-02 22:42_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:42_

It's not very hard but I don't know what assumptions it would break downstream.

---

_@zanieb reviewed on 2024-07-02 22:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:46_

Seems really annoying to nuke the environment before we solve the requirements. I guess we should create a couple tracking issues for these things.

---

_@charliermarsh reviewed on 2024-07-02 22:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:55_

We should probably solve before we do this.

---

_@charliermarsh reviewed on 2024-07-02 22:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:56_

https://github.com/astral-sh/uv/issues/4747

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:202 on 2024-07-02 22:57_

We actually already have an interpreter and everything so we could even solve before finding the environment etc.

---

_@charliermarsh reviewed on 2024-07-02 22:57_

---

_Merged by @charliermarsh on 2024-07-02 23:07_

---

_Closed by @charliermarsh on 2024-07-02 23:07_

---

_Branch deleted on 2024-07-02 23:07_

---
