---
number: 2109
title: D401 raised on classes and modules in v 0.0.230
type: issue
state: closed
author: niccolomineo
labels:
  - question
  - docstring
assignees: []
created_at: 2023-01-23T17:17:23Z
updated_at: 2023-01-24T01:38:53Z
url: https://github.com/astral-sh/ruff/issues/2109
synced_at: 2026-01-07T13:12:14-06:00
---

# D401 raised on classes and modules in v 0.0.230

---

_Issue opened by @niccolomineo on 2023-01-23 17:17_

Hi, just reporting that v. 0.0.230 raises a `D401` for classes and modules too. Is this on purpose?

---

_Comment by @charliermarsh on 2023-01-23 17:22_

Ah I thought we had fixed that in #2071! Do you mind linking a snippet to reproduce? Thanks :)

---

_Label `question` added by @charliermarsh on 2023-01-23 17:22_

---

_Label `docstring` added by @charliermarsh on 2023-01-23 17:22_

---

_Comment by @charliermarsh on 2023-01-23 17:50_

Oh wait, sorry, that commit didn't make it into v0.0.230.

---

_Comment by @charliermarsh on 2023-01-23 17:51_

I will cut v0.0.231 now.

---

_Comment by @edgarrmondragon on 2023-01-24 01:22_

@charliermarsh I think the `v0.0.231` release failed because github terminated it after 6 hours: https://github.com/charliermarsh/ruff/actions/runs/3989207120/jobs/6841354318

---

_Comment by @charliermarsh on 2023-01-24 01:28_

Oh thank you, let me try it again.

---

_Comment by @charliermarsh on 2023-01-24 01:38_

Ok, release is out, hopefully fixed!

---

_Closed by @charliermarsh on 2023-01-24 01:38_

---
