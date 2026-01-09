---
number: 7806
title: "Don't create .python-version in workspace subprojects"
type: issue
state: closed
author: bluss
labels:
  - enhancement
  - good first issue
assignees: []
created_at: 2024-09-30T09:17:45Z
updated_at: 2025-09-07T17:12:30Z
url: https://github.com/astral-sh/uv/issues/7806
synced_at: 2026-01-07T13:12:17-06:00
---

# Don't create .python-version in workspace subprojects

---

_Issue opened by @bluss on 2024-09-30 09:17_

I suggest that `.python-version` should not be created in subprojects in a workspace. The workspace root should contain this file already, and subprojects should not have them. The current version of uv creates this file when initing a new workspace member, and the user can/should delete the file afterwards - if I understand correctly.

uv 0.4.17

---

_Comment by @charliermarsh on 2024-09-30 12:48_

This makes sense to me.

---

_Label `enhancement` added by @charliermarsh on 2024-09-30 12:48_

---

_Label `good first issue` added by @zanieb on 2024-09-30 13:50_

---

_Comment by @dimitrivinet on 2024-09-30 16:02_

First time contributor here, didn't think it would send a message here 😅
Realized I don't have the correct format for the commit message, but how does this look ?

---

_Referenced in [astral-sh/uv#7815](../../astral-sh/uv/pulls/7815.md) on 2024-09-30 16:28_

---

_Comment by @konstin on 2024-10-04 13:22_

We don't have a particular format for commit messages, just try to write a short description of what got fixed or what got changed.

(Note that this issue is implemented by #7815)

---

_Comment by @zanieb on 2024-10-04 13:28_

And this already overlaps with some other in-flight work.

I'm actually not entirely sure about this, since a subproject can have different Python requirements it might make sense to be able to run it on another version? If we moved this metadata into the `pyproject.toml` would we still ban this?

---

_Comment by @bluss on 2024-10-04 13:35_

I think that `uv init` should not create this file because it's very redundant in subfolders of workspaces in my projects. To permit having it/using it - another issue?

---

_Comment by @zanieb on 2024-10-04 13:40_

Ah I see why I agreed on first read — yes I don't think `uv init` should create the redundant file. I think `uv python pin` in a subdirectory is "another question".

---

_Comment by @janie314 on 2025-09-07 14:13_

Is this implemented?

```
~/tmp/a # uv init
Initialized project `a`
~/tmp/a main # mkdir b
mkdir: created directory 'b'
~/tmp/a main # cd b
~/tmp/a/b main # uv init
Adding `b` as member of workspace `/home/janie/tmp/a`
Initialized project `b`
~/tmp/a/b main # ls -al
drwxr-xr-x@   - janie janie  7 Sep 09:11 -N .
drwxr-xr-x@   - janie janie  7 Sep 09:11 -N ..
.rw-r--r--@  79 janie janie  7 Sep 09:11 -N main.py
.rw-r--r--@ 147 janie janie  7 Sep 09:11 -N pyproject.toml
.rw-r--r--@   0 janie janie  7 Sep 09:11 -N README.md
~/tmp/a/b main # uv --version
uv 0.8.15
```

---

_Comment by @zanieb on 2025-09-07 17:12_

Yeah this looks implemented.

---

_Closed by @zanieb on 2025-09-07 17:12_

---

_Comment by @zanieb on 2025-09-07 17:12_

(Thanks!)

---
