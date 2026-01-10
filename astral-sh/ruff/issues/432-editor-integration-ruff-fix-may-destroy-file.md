---
number: 432
title: "Editor integration: `ruff --fix -` may destroy file contents when an update is available"
type: issue
state: closed
author: fsouza
labels:
  - bug
assignees: []
created_at: 2022-10-14T18:44:05Z
updated_at: 2022-10-17T22:01:46Z
url: https://github.com/astral-sh/ruff/issues/432
synced_at: 2026-01-10T01:22:38Z
---

# Editor integration: `ruff --fix -` may destroy file contents when an update is available

---

_Issue opened by @fsouza on 2022-10-14 18:44_

Use case: when formatting with the text editor, I send the file contents as stdin and replace the file contents with the stdout of `ruff`, but that output ends-up being the message about an update being available.

I just ran into it, I can't send a PR right now, but may be able to do it tomorrow night or Sunday. Reporting it just in case someone wants to work on it first :)

---

_Comment by @charliermarsh on 2022-10-14 19:07_

Oh hah, I can fix this :)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-14 19:07_

---

_Label `bug` added by @charliermarsh on 2022-10-14 19:07_

---

_Referenced in [astral-sh/ruff#433](../../astral-sh/ruff/pulls/433.md) on 2022-10-14 23:03_

---

_Closed by @charliermarsh on 2022-10-15 00:57_

---

_Comment by @fsouza on 2022-10-17 21:54_

Weird that I'm still seeing this issue (on ruff 0.0.79, I was offered an upgrade to 0.0.80 in the stdout). For now I'm setting `--quiet` in the editor config, but I'll also try to investigate to get more details.

---

_Comment by @fsouza on 2022-10-17 21:57_

Nevermind, this was a different issue 😁 

---

_Comment by @charliermarsh on 2022-10-17 22:01_

Let me know if I can help!

---
