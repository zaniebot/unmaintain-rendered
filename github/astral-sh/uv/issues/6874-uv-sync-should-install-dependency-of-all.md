---
number: 6874
title: uv sync should install dependency of all workspace members? 
type: issue
state: closed
author: jpambrun-vida
labels:
  - question
assignees: []
created_at: 2024-08-30T14:37:55Z
updated_at: 2025-08-05T07:53:58Z
url: https://github.com/astral-sh/uv/issues/6874
synced_at: 2026-01-07T13:12:17-06:00
---

# uv sync should install dependency of all workspace members? 

---

_Issue opened by @jpambrun-vida on 2024-08-30 14:37_

Prior to 0.4.0 I had a [project]-less top level pyproject.toml in my workspace. I liked that `uv sync` would install the union of all workspace members dependencies as well as top level dev-dependencies. This gave me a great DX with code completion/types/tests etc in all modules.

Now I was following the release note recommendation of adding a top level [project] table, but can't seem to get that behavior again.

* `uv sync` only installs top level dependencies
* if I add all workspace modules as dependency in the top level it works in dev, but later in CI building with `--package moduleA` I get errors that it can't find say moduleB since I only copy the needed modules in the docker context (to avoid rebuilding a module if changes were only in an unrelated modules).

If there a way to get `uv sync` to install all dependencies from all members uv can find without specifying them explicitly? like it does when [project] isn't present? Or, will the current [project]-less behavior remain unchanged? 

---

_Comment by @charliermarsh on 2024-08-30 14:40_

> ...but later in CI building with --package moduleA I get errors that it can't find say moduleB since I only copy the needed modules in the docker context (to avoid rebuilding a module if changes were only in an unrelated modules).

Are you looking for something like `--no-install-workspace`, to do a sync prior to copying over the code? It seems correct that `--package root` would fail if it depends on `member` and `member` doesn't exist.


---

_Label `question` added by @charliermarsh on 2024-09-01 15:54_

---

_Referenced in [astral-sh/uv#6935](../../astral-sh/uv/issues/6935.md) on 2024-09-02 12:09_

---

_Closed by @charliermarsh on 2024-09-03 01:19_

---

_Comment by @charliermarsh on 2024-09-03 01:19_

Closing based on my reply but happy to follow-up!

---

_Comment by @danielkonecny on 2025-07-16 14:07_

Since OP didn't follow-up, I might do. If I understand the question correctly (and with that, I would like to see this implemented as well), OP would like to have `uv sync` install all dependencies of all workspace members. Right now, only the root dependencies are installed. I hope I'm not missing any special settings that allow for that, I'm only beginning to use workspaces.

---

_Comment by @zanieb on 2025-07-16 14:16_

I think you just want `--all-packages`

---

_Comment by @danielkonecny on 2025-08-05 07:53_

> I think you just want `--all-packages`

You are right, thank you!

---
