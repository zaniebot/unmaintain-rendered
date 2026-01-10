---
number: 11515
title: QUESTION // install requirements for existing project
type: issue
state: closed
author: nfparham
labels:
  - question
assignees: []
created_at: 2025-02-14T17:35:12Z
updated_at: 2025-02-14T18:52:25Z
url: https://github.com/astral-sh/uv/issues/11515
synced_at: 2026-01-10T01:25:06Z
---

# QUESTION // install requirements for existing project

---

_Issue opened by @nfparham on 2025-02-14 17:35_

### Question

It is not clear from the docs how to install the requirements of an existing project using uv.lock rather than requirements.txt. The docs clearly state that both should not be maintained, but how do I install all dependencies using uv.lock like I would have done with requirements.txt?

### Platform

*

### Version

*

---

_Label `question` added by @nfparham on 2025-02-14 17:35_

---

_Renamed from "QUESTION // install existing project requirements" to "QUESTION // install requirements for existing project" by @nfparham on 2025-02-14 17:36_

---

_Comment by @zanieb on 2025-02-14 17:53_

With `uv run`, `uv sync`, or `uv export | uv pip install -r -`

---

_Comment by @nfparham on 2025-02-14 18:41_

Sure enough, user error per usual! Thanks so much @zanieb 

---

_Comment by @zanieb on 2025-02-14 18:52_

No problem! We need more `uv sync` documentation :)

---

_Closed by @zanieb on 2025-02-14 18:52_

---
