```yaml
number: 15553
title: "Allow `uv format` in unmanaged projects"
type: pull_request
state: merged
author: jorgehermo9
labels: []
assignees: []
merged: true
base: main
head: fix/format-project-not-discovered
created_at: 2025-08-27T16:29:25Z
updated_at: 2025-09-05T18:14:41Z
url: https://github.com/astral-sh/uv/pull/15553
synced_at: 2026-01-10T06:44:33Z
```

# Allow `uv format` in unmanaged projects

---

_Pull request opened by @jorgehermo9 on 2025-08-27 16:29_

Closes #15550

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:49 on 2025-08-27 16:30_

I decided to fallback to `project_dir` only on those three variants. Inspired from https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/venv.rs#L97

---

_@jorgehermo9 reviewed on 2025-08-27 16:30_

---

_@jorgehermo9 reviewed on 2025-08-27 16:33_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:52 on 2025-08-27 16:33_

I'm not sure on the behaviour in this case. Should we propagate the error to the caller (previous behaviour), or in this case, should we `warn_user!`  about the error and fallback to `project_dir` (as the previous branch does)?

I prefer to not do "magic fallbacks" when there is an error such as this (for example, malformed pyproject.toml) and to just propagate them to the user instead of continue with the command. Let me know your thoughts please.

In https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/venv.rs#L97 such fallback is done, logging a warning in these cases and continue working using defaults


---

_@jorgehermo9 reviewed on 2025-08-27 16:35_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:46 on 2025-08-27 16:35_

had to `.to_owned()`... If I instead do `match &project`, then the `Err(err) => return Err(err.into()),` would fail as there is no `From <&WorkspaceError> for anyhow::Error` (there exist such implementation for owned `WorkspaceError` ,but not for borrowed). Therefore we need the owned version (`match project`) and just return `proj.root()` would also have lifetime issues due to the move semantics of `match project`

---

_@jorgehermo9 reviewed on 2025-08-27 16:36_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:241 on 2025-08-27 16:36_

Added a failing test, as I see there was no one previously. Not sure if I should cover more cases or just this one

---

_@jorgehermo9 reviewed on 2025-08-27 16:37_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:262 on 2025-08-27 16:37_

It is weird that this error is reported twice... But I see this same happens in other failing tests, is this intended?

---

_@zanieb reviewed on 2025-08-27 17:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:262 on 2025-08-27 17:37_

Is the second parse error from ruff?

---

_@jorgehermo9 reviewed on 2025-08-27 17:41_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:262 on 2025-08-27 17:41_

nope, from the err propagation at `format` function in format.rs. The first one is the one I'm not sure where is logged

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:262 on 2025-08-27 17:45_

I would presume the first one is when we load global settings? It seems problematic, but out of scope for fixing here if there are other snapshots with the same problem.

---

_@zanieb reviewed on 2025-08-27 17:45_

---

_@zanieb reviewed on 2025-08-27 17:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:52 on 2025-08-27 17:47_

It seems okay to fail here, unless we find a use-case it's blocking. It's much harder to go the other direction :)

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:44 on 2025-08-27 17:47_

nit: I might call this `target_dir` instead?

---

_@zanieb reviewed on 2025-08-27 17:47_

---

_@zanieb reviewed on 2025-08-27 17:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:44 on 2025-08-27 17:49_

If we don't re-use the `project` binding I might just do `match VirtualProject::discover(...)` directly.

---

_@zanieb approved on 2025-08-27 17:49_

---

_@jorgehermo9 reviewed on 2025-08-27 17:52_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:262 on 2025-08-27 17:52_

okay! I'll debug that later and if I found which is the origi, I'll create an issue for it! (ping me if you create it first)

---

_@jorgehermo9 reviewed on 2025-08-27 17:57_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:44 on 2025-08-27 17:57_

good catch! I'll update in a few hours (I'm currently away from home)

---

_@jorgehermo9 reviewed on 2025-08-27 17:57_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:44 on 2025-08-27 17:57_

I like it, more semantic!

---

_Comment by @jorgehermo9 on 2025-08-28 21:44_

sorry @zanieb I couldn't address the comments yet. I will work on it as soon as I can. I hope tomorrow/next day

---

_Comment by @jorgehermo9 on 2025-09-05 17:26_

Hi @zanieb Everything addressed!

CC @younver

---

_@zanieb reviewed on 2025-09-05 17:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:49 on 2025-09-05 17:51_

```suggestion
            // If there is a problem finding a project, we just use the provided directory,
            // e.g., for unmanaged projects
```

---

_@zanieb approved on 2025-09-05 18:14_

---

_Merged by @zanieb on 2025-09-05 18:14_

---

_Closed by @zanieb on 2025-09-05 18:14_

---
