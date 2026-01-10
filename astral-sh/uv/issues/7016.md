```yaml
number: 7016
title: "Automatic Git Repository Initialization for `uv init` Command"
type: issue
state: closed
author: akmalsoliev
labels:
  - enhancement
assignees: []
created_at: 2024-09-04T13:23:53Z
updated_at: 2025-02-18T14:08:38Z
url: https://github.com/astral-sh/uv/issues/7016
synced_at: 2026-01-10T03:50:30Z
```

# Automatic Git Repository Initialization for `uv init` Command

---

_Issue opened by @akmalsoliev on 2024-09-04 13:23_

Implement a feature that automatically initializes a Git repository and generates a `.gitignore` file when the `uv init` command is executed. This functionality should mirror the behavior of `cargo new` or `cargo init --vcs` in the Cargo package manager for Rust.

---

_Comment by @zanieb on 2024-09-04 13:25_

See https://github.com/astral-sh/uv/pull/5476

---

_Comment by @akmalsoliev on 2024-09-04 13:36_

> See #5476

great! looking forward to it. Should I close this issue or keep it open till the PR is merged? 

---

_Closed by @akmalsoliev on 2024-09-04 13:36_

---

_Comment by @zanieb on 2024-09-04 13:56_

We can track here — not sure if there's another issue 

---

_Reopened by @zanieb on 2024-09-04 13:56_

---

_Comment by @charliermarsh on 2024-09-26 02:47_

Done!

---

_Closed by @charliermarsh on 2024-09-26 02:47_

---

_Label `enhancement` added by @charliermarsh on 2024-09-26 02:47_

---

_Comment by @debnath-d on 2025-02-18 14:08_

The currently used `.gitignore` file is quite bare-bones. Would you consider using https://github.com/github/gitignore/blob/main/Python.gitignore instead?

---
