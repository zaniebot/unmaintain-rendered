```yaml
number: 3657
title: "Add initial implementation of `uv tool run`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-run
created_at: 2024-05-19T14:11:58Z
updated_at: 2024-05-21T19:51:32Z
url: https://github.com/astral-sh/uv/pull/3657
synced_at: 2026-01-12T16:05:46Z
```

# Add initial implementation of `uv tool run`

---

_@zanieb_

This is mostly a shorter version of `uv run` that infers a requirement name from the command. The main goal here is to do the smallest amount of work necessary to get #3560 started.

Closes #3613 

e.g.
```shell
$ uv tool run -- ruff check
warning: `uv tool run` is experimental and may change without warning.
Resolved 1 package in 34ms
Installed 1 package in 2ms
 + ruff==0.4.4
error: Failed to parse example.py:1:5: Expected an expression
example.py:1:5: E999 SyntaxError: Expected an expression
Found 1 error.
```

---

_Label `preview` added by @zanieb on 2024-05-19 14:11_

---

_@zanieb reviewed on 2024-05-19 14:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:188 on 2024-05-19 14:13_

@charliermarsh where do you think we should put this?

---

_@charliermarsh reviewed on 2024-05-19 18:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:188 on 2024-05-19 18:17_

Probably in the module root.

---

_@charliermarsh reviewed on 2024-05-19 18:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:63 on 2024-05-19 18:18_

Ultimately, will these be installed in some sort of global directory that's also exposed on PATH, either by adding at install time or via some kind of `ensurepath`?

---

_@charliermarsh reviewed on 2024-05-19 18:21_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:63 on 2024-05-19 18:21_

Ok, I see this in https://github.com/astral-sh/uv/issues/3560 now. If the tool is already installed in the user environment, it will be used, but `uv tool run` on its own will not _install_ the tool. Is that correct?

---

_@zanieb reviewed on 2024-05-19 18:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:188 on 2024-05-19 18:32_

Maybe we should have environment update code in the `virtualenv` crate or some sort of dedicated crate for managing environments?

---

_@zanieb reviewed on 2024-05-19 18:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:63 on 2024-05-19 18:34_

Yeah `uv tool run` will not install a tool. It could cache the environment and will support detecting user-installed and project-level tools but we don't have those yet here. I've already started a branch for `uv tool install` — it's just much more complex as a starting point.

---

_@zanieb reviewed on 2024-05-19 19:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:188 on 2024-05-19 19:41_

Moving this to #3659 

---

_Marked ready for review by @zanieb on 2024-05-21 15:23_

---

_Merged by @zanieb on 2024-05-21 19:51_

---

_Closed by @zanieb on 2024-05-21 19:51_

---

_Branch deleted on 2024-05-21 19:51_

---
