---
number: 3063
title: Installing packages from custom pypi server fails on 0.1.32
type: issue
state: closed
author: bnaul
labels:
  - bug
  - external
assignees: []
created_at: 2024-04-16T15:11:52Z
updated_at: 2025-11-08T19:28:24Z
url: https://github.com/astral-sh/uv/issues/3063
synced_at: 2026-01-07T13:12:17-06:00
---

# Installing packages from custom pypi server fails on 0.1.32

---

_Issue opened by @bnaul on 2024-04-16 15:11_

Highly likely that the root issue is on our end but figured it's still worth an issue since others might be facing the same issue soon: upon upgrading to `uv==0.1.32`, our docker builds started failing with
```
error: HTTP status server error (502 Bad Gateway) for url
```
Rolling back to `0.1.31` fixed things. 

We `UV_INDEX_URL=... pip install -r requirements.txt` with a couple (3?) of packages coming from a self-hosted `pypiserver` instance, and the rest (400?) are publicly available on PyPI. No obvious issues with the health of our server, CPU load is still extremely low, can't find anything noteworthy in the logs. 

I `git bisect`ed my way to #2817 and found that any commits before this change succeed. That's about all I've been able to find but if there's any other `uv` or `pypiserver` output that would be of interest let me know and I'll post it!

---

_Comment by @zanieb on 2024-04-22 19:18_

Hey Brett! Good to see you.

Sorry this issue fell through the cracks, are you still encountering this on the latest version? I think a lot changed in our network stack with #2817, seems possible we made a mistake or that there's a regression upstream.


---

_Label `bug` added by @zanieb on 2024-04-22 19:18_

---

_Label `upstream` added by @zanieb on 2024-04-22 19:18_

---

_Comment by @bnaul on 2024-04-22 21:05_

Hi @zanieb 👋 yes, I just checked and we still encounter this on 0.1.36. Don't have many other details to share but a couple other things:
- `uv pip install <a few packages>` works fine, it's only installing our full set of requirements that triggers the issue
- `uv pip compile` also reliably triggers the issue for su

---

_Comment by @zanieb on 2024-04-22 22:05_

FWIW we've used pypiserver for some testing and I've seen pretty consistent errors at high levels of parallelism. We may just need to let people turn down our network concurrency.

---

_Comment by @zanieb on 2025-11-08 19:28_

We now support configuring concurrent network requests: https://docs.astral.sh/uv/reference/environment/#uv_concurrent_downloads

---

_Closed by @zanieb on 2025-11-08 19:28_

---
