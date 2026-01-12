```yaml
number: 5956
title: Display portable paths in posix venv activation commands
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: zb/portable-activate
created_at: 2024-08-09T13:35:20Z
updated_at: 2025-03-02T17:52:13Z
url: https://github.com/astral-sh/uv/pull/5956
synced_at: 2026-01-12T16:07:07Z
```

# Display portable paths in posix venv activation commands

---

_@zanieb_

Closes #5950

---

_Label `bug` added by @zanieb on 2024-08-09 13:35_

---

_@charliermarsh approved on 2024-08-09 13:36_

---

_Comment by @charliermarsh on 2024-08-09 13:36_

Will this get correctly detected as Posix on Windows Git Bash?

---

_Comment by @charliermarsh on 2024-08-09 13:36_

I can probably test it.

---

_Comment by @zanieb on 2024-08-09 13:53_

I'm pretty sure, but I'd appreciate if you tested it. 

---

_Comment by @charliermarsh on 2024-08-09 14:50_

Will do.

---

_Comment by @charliermarsh on 2024-08-09 15:16_

Confirmed that this works.

---

_Label `windows` added by @charliermarsh on 2024-08-09 15:17_

---

_Marked ready for review by @zanieb on 2024-08-09 16:27_

---

_Merged by @zanieb on 2024-08-09 16:28_

---

_Closed by @zanieb on 2024-08-09 16:28_

---

_Branch deleted on 2024-08-09 16:28_

---

_Comment by @chourroutm on 2025-03-02 17:39_

Hi! Sorry, I don't understand how to have uv recognized for the Git for Windows Bash. Do I need to append uv binaries to the Windows env var PATH?

I installed it in Windows PowerShell but get 

> bash: uv: command not found

in Git Bash.

My workaround was to append `PATH=$PATH:$HOME/.local/bin/` in my ~/.bashrc

---
