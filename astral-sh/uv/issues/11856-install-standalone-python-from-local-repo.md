---
number: 11856
title: Install standalone Python from local repo
type: issue
state: closed
author: etiennebonnafoux
labels:
  - question
assignees: []
created_at: 2025-02-28T14:35:50Z
updated_at: 2025-02-28T14:59:24Z
url: https://github.com/astral-sh/uv/issues/11856
synced_at: 2026-01-10T01:25:12Z
---

# Install standalone Python from local repo

---

_Issue opened by @etiennebonnafoux on 2025-02-28 14:35_

### Question

Hello,

For security reason, some devellopment plateform are isolated from Internet. With `uv` one can already install dependenie from a local package repository. But installation of python version come from the github [repo](https://github.com/astral-sh/python-build-standalone) of the standalone project. 
Can we also give `uv` instruction to install python from a local github/gitlab ? Something like :
```
[tool.uv.sources]
python = { index = "python" }

[[tool.uv.index]]
name = "python"
url = "https://secure.gitlab/python-build-standalone"
```

Thanks for all

### Platform

all

### Version

_No response_

---

_Label `question` added by @etiennebonnafoux on 2025-02-28 14:35_

---

_Comment by @chrisrodrigue on 2025-02-28 14:49_

`python-build-standalone` is just the build scripts… what I think you want is the actual Python binaries, which you might not want to keep in GitLab (unless you were using LFS or something).

I opened https://github.com/astral-sh/uv/issues/11746 to address the need to create secure Python distributions  that are fully offline capable.

---

_Comment by @etiennebonnafoux on 2025-02-28 14:59_

Thanks I will follow your issue.

---

_Closed by @etiennebonnafoux on 2025-02-28 14:59_

---
