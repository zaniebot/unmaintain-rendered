---
number: 14833
title: "How do I change the default Python version used for `uv init`?"
type: issue
state: closed
author: mzhu22
labels:
  - question
assignees: []
created_at: 2025-07-22T21:07:27Z
updated_at: 2025-07-22T21:28:11Z
url: https://github.com/astral-sh/uv/issues/14833
synced_at: 2026-01-10T01:25:49Z
---

# How do I change the default Python version used for `uv init`?

---

_Issue opened by @mzhu22 on 2025-07-22 21:07_

### Question

When I `uv init` a new project, the Python version is always set to 3.10 (probably due to some change I made in the past). 

How do I change this? I understand that I can `uv python pin` in a project at any time, but would like to be able to change the default somehow. Thank you!

### Platform

_No response_

### Version

uv 0.8.2

---

_Label `question` added by @mzhu22 on 2025-07-22 21:07_

---

_Comment by @zanieb on 2025-07-22 21:18_

`uv python pin --global <version>` should work

---

_Comment by @zanieb on 2025-07-22 21:19_

(You can use `uv python pin` to view your current pin or `uv python find -v` to see where we're selecting an interpreter from)

---

_Comment by @mzhu22 on 2025-07-22 21:28_

Wonderful, thank you!

---

_Closed by @mzhu22 on 2025-07-22 21:28_

---
