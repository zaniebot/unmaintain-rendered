```yaml
number: 9585
title: Include base installation directory in uv run PATH
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2024-12-02T22:01:52Z
updated_at: 2024-12-02T23:05:34Z
url: https://github.com/astral-sh/uv/pull/9585
synced_at: 2026-01-12T16:08:53Z
```

# Include base installation directory in uv run PATH

---

_@charliermarsh_

## Summary

On Windows, non-virtual environments put the `python.exe` in the top-level of the installation directory, rather than in `Scripts`. This PR adds those paths to `PATH` in `uv run` and `uv tool run`.

Closes https://github.com/astral-sh/uv/issues/9574#issuecomment-2512217110.


---

_Label `bug` added by @charliermarsh on 2024-12-02 22:01_

---

_Label `windows` added by @charliermarsh on 2024-12-02 22:01_

---

_@zanieb reviewed on 2024-12-02 22:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:10_

I'm sort of hesitant to do this so broadly, couldn't this add something like `/usr/bin` to the `PATH`? I would find that surprising. That example is a bit contrived, but it could be an arbitrary directory containing a link to the interpreter right? Should we just be special-casing the Python executable (e.g., by invoking the full path to the interpreter) instead of adding its parent directory to the `PATH`?

---

_@zanieb reviewed on 2024-12-02 22:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:10_

Ah sorry that suggestion doesn't resolve the linked issue, in which you'd expect the executable to be on the `PATH` in a child shell. Hm.

---

_@zanieb reviewed on 2024-12-02 22:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:12_

In the ephemeral environment case we should never need to do this, right?

---

_@zanieb reviewed on 2024-12-02 22:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:13_

Another possibility is #5839 

---

_@charliermarsh reviewed on 2024-12-02 22:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:13_

Hmm, I don't think that's sufficient. There are other executables in the top-level too, right? I think this is just correct, at least on Windows. We could limit it to Windows?

---

_@charliermarsh reviewed on 2024-12-02 22:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:13_

Yeah good call. That will always be virtual.

---

_@charliermarsh reviewed on 2024-12-02 22:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:13_

We could also choose to limit this to Windows, non-virtual environments.

---

_Comment by @charliermarsh on 2024-12-02 22:14_

I tested this on my machine and confirmed that the path is added for non-virtual environments and omitted otherwise.

---

_@zanieb reviewed on 2024-12-02 22:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1026 on 2024-12-02 22:16_

I'm more comfortable limiting this to Windows.

---

_Comment by @charliermarsh on 2024-12-02 22:48_

Limited to Windows, and to `uv run` (not `uv tool run`).

---

_@zanieb approved on 2024-12-02 22:55_

---

_Merged by @charliermarsh on 2024-12-02 23:05_

---

_Closed by @charliermarsh on 2024-12-02 23:05_

---

_Branch deleted on 2024-12-02 23:05_

---
