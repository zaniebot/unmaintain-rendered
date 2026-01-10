```yaml
number: 8609
title: "Support `uv export --script`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-10-28T00:03:09Z
updated_at: 2025-01-08T21:48:55Z
url: https://github.com/astral-sh/uv/issues/8609
synced_at: 2026-01-10T04:27:58Z
```

# Support `uv export --script`

---

_Issue opened by @charliermarsh on 2024-10-28 00:03_

You should be able to point to a PEP 723 script to get a requirements.txt of the resolved dependencies.

---

_Label `enhancement` added by @charliermarsh on 2024-10-28 00:03_

---

_Label `help wanted` added by @charliermarsh on 2024-10-28 00:03_

---

_Comment by @manzt on 2024-10-28 17:26_

This would be awesome (especially for the jupyter notebook stuff around uv I'm tinkering with). 

I made [an attempt](https://github.com/astral-sh/uv/compare/main...manzt:uv:manzt/export?expand=1) at a PR, but got a bit stuck. It seems like workspaces are pretty connected with `uv export` right now and wasn't sure of the right way to push forward (i.e., create a virtual workspace or special case scripts). Maybe some of the code is useful.



---

_Comment by @malikrohail on 2024-10-29 01:34_

can i work on this? (pull req)

---

_Comment by @zanieb on 2024-10-29 01:40_

@malikrohail feel free, but note @manzt has some work in progress and said it didn't seem trivial.

---

_Comment by @manzt on 2024-10-29 01:42_

Probably won't have time to touch for a bit so go right ahead! Thought I'd share WIP in case any of the code is helpful.

---

_Comment by @malikrohail on 2024-10-29 01:48_

yes that would be helpful and fasten things up, pls share ur progress

---

_Comment by @zanieb on 2024-10-29 02:09_

(their progress is linked above)

---

_Comment by @malikrohail on 2024-10-29 14:02_

sounds good, will work on it later today. 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-25 19:18_

---

_Closed by @charliermarsh on 2025-01-08 21:48_

---

_Closed by @charliermarsh on 2025-01-08 21:48_

---
