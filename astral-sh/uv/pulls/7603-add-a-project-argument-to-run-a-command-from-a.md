```yaml
number: 7603
title: "Add a `--project` argument to run a command from a project"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/project
created_at: 2024-09-20T21:08:33Z
updated_at: 2024-09-21T20:19:50Z
url: https://github.com/astral-sh/uv/pull/7603
synced_at: 2026-01-10T12:53:51Z
```

# Add a `--project` argument to run a command from a project

---

_Pull request opened by @charliermarsh on 2024-09-20 21:08_

## Summary

`uv run --project ./path/to/project` now uses the provided directory as the starting point for any file discovery. However, relative paths are still resolved relative to the current working directory.

Closes https://github.com/astral-sh/uv/issues/5613.


---

_@charliermarsh reviewed on 2024-09-20 21:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:1366 on 2024-09-20 21:09_

I think this is correct... because the project environment _is_ at `project/.venv` relative to the current working directory.

---

_Review requested from @zanieb by @charliermarsh on 2024-09-20 21:14_

---

_Label `enhancement` added by @charliermarsh on 2024-09-20 21:14_

---

_Label `cli` added by @charliermarsh on 2024-09-20 21:14_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-20 21:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:250 on 2024-09-21 13:58_

Ignored in the `uv pip` interface though?

Should we note this affects project environment discovery too?

---

_@zanieb reviewed on 2024-09-21 13:58_

---

_@zanieb approved on 2024-09-21 13:58_

---

_Review comment by @bluss on `crates/uv-cli/src/lib.rs`:246 on 2024-09-21 14:09_

```suggestion
    /// up the directory tree from the project root, while other command-line arguments will be resolved relative
    /// to the current working directory.
```

This might be more clear. Some users could take "relative paths" to mean something else, like paths in project config? I'm not sure about the exact best way to say it.

---

_@bluss reviewed on 2024-09-21 14:11_

---

_Merged by @charliermarsh on 2024-09-21 20:19_

---

_Closed by @charliermarsh on 2024-09-21 20:19_

---

_Branch deleted on 2024-09-21 20:19_

---
