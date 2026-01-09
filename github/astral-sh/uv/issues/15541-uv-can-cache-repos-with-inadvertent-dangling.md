---
number: 15541
title: uv can cache repos with inadvertent dangling symlinks that break uv cache clean
type: issue
state: closed
author: Owen-OptiGrid
labels:
  - bug
assignees: []
created_at: 2025-08-27T01:11:28Z
updated_at: 2025-08-27T22:34:56Z
url: https://github.com/astral-sh/uv/issues/15541
synced_at: 2026-01-07T13:12:19-06:00
---

# uv can cache repos with inadvertent dangling symlinks that break uv cache clean

---

_Issue opened by @Owen-OptiGrid on 2025-08-27 01:11_

### Summary

We have a package in our org that we import as a git repo source into our other repos. In our package code I symlinked AGENTS.md to .github/copilot-instructions.md as we use the Codex coding agent and GitHub Copilot. The issue was in the uv cache for our package all `.` prefixed folders were excluded (so no .github directory) which left my AGENTS.md as a dangling symlink. Which didn't cause any issues until I tried to run `uv cache clean` which errored with an os error.

<img width="2012" height="132" alt="Image" src="https://github.com/user-attachments/assets/e49d717a-8202-4012-9226-ac1944669d83" />

I now know not to symlink to files in `.` prefixed directories like this... although I do wonder if uv could've handled this issue more gracefully.

### Platform

Windows 11 x86_64

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

### Python version

Python 3.13.7

---

_Label `bug` added by @Owen-OptiGrid on 2025-08-27 01:11_

---

_Comment by @charliermarsh on 2025-08-27 01:26_

Thanks, agreed we should probably do better in this case.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-27 01:46_

---

_Referenced in [astral-sh/uv#15543](../../astral-sh/uv/pulls/15543.md) on 2025-08-27 02:02_

---

_Closed by @charliermarsh on 2025-08-27 11:43_

---

_Comment by @Owen-OptiGrid on 2025-08-27 22:34_

That's amazingly fast response time, thanks @charliermarsh !

---
