```yaml
number: 5977
title: "Warn when project-specific settings are passed to non-project `uv run` commands"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/warn-run
created_at: 2024-08-09T20:22:32Z
updated_at: 2024-08-09T23:10:35Z
url: https://github.com/astral-sh/uv/pull/5977
synced_at: 2026-01-10T13:31:54Z
```

# Warn when project-specific settings are passed to non-project `uv run` commands

---

_Pull request opened by @charliermarsh on 2024-08-09 20:22_

## Summary

Closes https://github.com/astral-sh/uv/issues/5856.


---

_Label `cli` added by @charliermarsh on 2024-08-09 20:22_

---

_Label `preview` added by @charliermarsh on 2024-08-09 20:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-09 20:22_

---

_@charliermarsh reviewed on 2024-08-09 20:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:261 on 2024-08-09 20:23_

We could enforce these _specific_ warnings via Clap, and make them errors. But the others must be done at runtime, because we don't _know_ if we'll discover a project when we first parse the arguments. So I just did them all as warnings for consistency.

---

_@charliermarsh reviewed on 2024-08-09 21:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:192 on 2024-08-09 21:51_

I will change this to show specific CLI arguments when I do https://github.com/astral-sh/uv/issues/5855 (I'll need access to the args to fix that).

---

_@zanieb reviewed on 2024-08-09 22:17_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:332 on 2024-08-09 22:17_

Python scripts _with_ inline metadata — not all Python scripts.

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:1155 on 2024-08-09 22:17_

Minor preference for "when used with" instead of "alongside".

---

_@zanieb reviewed on 2024-08-09 22:17_

---

_@zanieb reviewed on 2024-08-09 22:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:268 on 2024-08-09 22:18_

Extras were "requested"?

---

_Comment by @zanieb on 2024-08-09 22:18_

Is this the only interface this applies to? Thank you!

---

_Comment by @charliermarsh on 2024-08-09 22:57_

Arguably also `no_workspace` in `uv init` (warn if you're not in a workspace).


---

_Merged by @charliermarsh on 2024-08-09 23:10_

---

_Closed by @charliermarsh on 2024-08-09 23:10_

---

_Branch deleted on 2024-08-09 23:10_

---
