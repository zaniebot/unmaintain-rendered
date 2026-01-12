```yaml
number: 8025
title: Cache Python version downloads
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2024-10-08T23:08:53Z
updated_at: 2025-01-23T17:26:20Z
url: https://github.com/astral-sh/uv/issues/8025
synced_at: 2026-01-12T15:59:18Z
```

# Cache Python version downloads

---

_@zanieb_

I find myself sitting around a lot waiting for these to download again. It seems common enough to try uninstalling and installing versions that we should just cache the distributions?

---

_Comment by @FishAlchemist on 2024-10-10 19:44_

This question is about ``uv python install``, right? I think I understand it correctly.
I've used it in places with poor network connections, so I'm still quite worried that if the installation fails, I'll have to download it again.
I was also thinking about trying to use a  flash drive(USB flash) with a compressed CPython file and then installing Python from that compressed file using uv python install.

(Although it may seem a bit extreme, the network conditions at the time were so poor that the download speed was often less than 10KB per second, and sometimes even as low as 1KB per second.)


---

_Comment by @chrisrodrigue on 2024-10-10 22:02_

It would would be super cool if the distribution was encoded in the uv binary of the specified platform! 

One wouldn't need to download python at all. `uv` would be the whole shebang (no pun intended).

---

_Comment by @zanieb on 2024-10-11 02:12_

> This question is about uv python install, right? 

Yep

> It would would be super cool if the distribution was encoded in the uv binary of the specified platform!

I'm not sure we want to make the uv binary bigger, the distributions aren't super lightweight. It could be kind of nice to have a single version around 🤔 something to consider. Probably best not mixed with the goal of this issue though.

---

_Comment by @hutch3232 on 2025-01-23 17:26_

I came here to create this same issue. Having python downloads cached (and hopefully at `UV_CACHE_DIR` which I'm already setting to a persistent location) would be a great help/time saver.

I work in ephemeral environments (docker + kubernetes) so I would need to regularly re-download the binaries. They're not huge, but it does take some time. I find myself currently sticking with the system installed python version, despite it being older (3.9), because I'm impatient with python download times. `uv` python package install times have already spoiled me...

---
