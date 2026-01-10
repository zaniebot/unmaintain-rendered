---
number: 2854
title: "`S608` Triggered on Exception"
type: issue
state: closed
author: colin99d
labels:
  - wontfix
assignees: []
created_at: 2023-02-13T12:44:52Z
updated_at: 2023-02-13T14:10:24Z
url: https://github.com/astral-sh/ruff/issues/2854
synced_at: 2026-01-10T01:22:41Z
---

# `S608` Triggered on Exception

---

_Issue opened by @colin99d on 2023-02-13 12:44_

I am getting an `S608` error on the following line of code:
`raise Exception(f"Select a valid asset from {', '.join(ASSETS)}")`

There is no use of SQL in the file.

---

_Label `wontfix` added by @charliermarsh on 2023-02-13 13:58_

---

_Comment by @charliermarsh on 2023-02-13 13:58_

I think this is probably consistent with Bandit unfortunately.

---

_Closed by @charliermarsh on 2023-02-13 13:58_

---

_Referenced in [PyCQA/bandit#984](../../PyCQA/bandit/issues/984.md) on 2023-02-13 14:09_

---

_Comment by @spaceone on 2023-02-13 14:10_

→ https://github.com/PyCQA/bandit/issues/984#issuecomment-1428001342

---
