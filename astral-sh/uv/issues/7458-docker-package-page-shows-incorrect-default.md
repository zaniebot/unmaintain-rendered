---
number: 7458
title: Docker package page shows incorrect default suggestion
type: issue
state: closed
author: zanieb
labels:
  - releases
assignees: []
created_at: 2024-09-17T13:20:52Z
updated_at: 2024-09-20T19:05:52Z
url: https://github.com/astral-sh/uv/issues/7458
synced_at: 2026-01-10T01:24:15Z
---

# Docker package page shows incorrect default suggestion

---

_Issue opened by @zanieb on 2024-09-17 13:20_

<img width="783" alt="Screenshot 2024-09-17 at 8 20 00 AM" src="https://github.com/user-attachments/assets/19fe9fd0-e99c-44a3-a4d9-3e9760fc4d32">

I would expect at least the latest Python version? And probably not a specific variant?

This seems like it might be a pain to fix.

cc @samypr100 

---

_Comment by @samypr100 on 2024-09-17 15:53_

The API shows them in the order they're published :(
We'd have to disable the matrix approach if we want some kind of order.

---

_Comment by @zanieb on 2024-09-17 15:55_

Can we just publish the "right" one a second time after all the rest?

---

_Comment by @samypr100 on 2024-09-17 15:56_

Yes, I was thinking about that too, adding a final job to re-publish.

---

_Label `releases` added by @zanieb on 2024-09-17 16:31_

---

_Comment by @samypr100 on 2024-09-18 03:59_

I gave it a try and it didn't quite work as I expected, GitHub seems to sort based on when digests get pushed, so re-doing the `docker-publish` job at the end (which only updates the tags) didn't help and we'd have to rebuild a new image to have the digests change. It seems like a limitation on Github's side to not allow configurability of which tag gets featured. I'll keep thinking about it.

---

_Referenced in [astral-sh/uv#7568](../../astral-sh/uv/pulls/7568.md) on 2024-09-20 01:25_

---

_Comment by @samypr100 on 2024-09-20 01:27_

> I gave it a try and it didn't quite work as I expected, GitHub seems to sort based on when digests get pushed, so re-doing the `docker-publish` job at the end (which only updates the tags) didn't help and we'd have to rebuild a new image to have the digests change. It seems like a limitation on Github's side to not allow configurability of which tag gets featured. I'll keep thinking about it.

I found a trick by leveraging annotations proposed in #7568 by which makes this achievable.

---

_Closed by @zanieb on 2024-09-20 19:05_

---
