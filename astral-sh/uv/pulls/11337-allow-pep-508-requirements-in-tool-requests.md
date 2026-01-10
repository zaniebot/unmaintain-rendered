```yaml
number: 11337
title: Allow PEP 508 requirements in tool requests
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2025-02-08T02:41:15Z
updated_at: 2025-02-12T00:06:42Z
url: https://github.com/astral-sh/uv/pull/11337
synced_at: 2026-01-10T11:10:35Z
```

# Allow PEP 508 requirements in tool requests

---

_Pull request opened by @charliermarsh on 2025-02-08 02:41_

## Summary

This allows previously-unsupported patterns like:

- `uvx ruff>=0.5.0`
- `uvx flask[dotenv]`

Closes https://github.com/astral-sh/uv/issues/6296.


---

_Review requested from @zanieb by @charliermarsh on 2025-02-08 02:41_

---

_Label `enhancement` added by @charliermarsh on 2025-02-08 02:41_

---

_Label `cli` added by @charliermarsh on 2025-02-08 02:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/mod.rs`:182 on 2025-02-08 02:42_

I'm considering refactoring this whole enum, but this seems fine for now.

---

_@charliermarsh reviewed on 2025-02-08 02:42_

---

_@charliermarsh reviewed on 2025-02-08 02:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/mod.rs`:182 on 2025-02-08 02:47_

For context, this is like, "Given `ruff<0.5.0`, find `ruff`", i.e., the command name.

---

_@zanieb reviewed on 2025-02-11 14:39_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:104 on 2025-02-11 14:39_

Or version? :) future goal..

---

_@zanieb approved on 2025-02-11 14:40_

---

_Merged by @charliermarsh on 2025-02-12 00:06_

---

_Closed by @charliermarsh on 2025-02-12 00:06_

---

_Branch deleted on 2025-02-12 00:06_

---
