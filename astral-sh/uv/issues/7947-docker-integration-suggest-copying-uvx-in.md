---
number: 7947
title: "Docker integration: Suggest COPYing `uvx` in addition to `uv`"
type: issue
state: closed
author: salty-horse
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-10-06T07:07:59Z
updated_at: 2024-10-14T18:23:12Z
url: https://github.com/astral-sh/uv/issues/7947
synced_at: 2026-01-10T01:24:21Z
---

# Docker integration: Suggest COPYing `uvx` in addition to `uv`

---

_Issue opened by @salty-horse on 2024-10-06 07:07_

The [Docker integration guide](https://docs.astral.sh/uv/guides/integration/docker/) suggests "copying the binary from the official distroless Docker image":
```
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
```

For the "full experience", I think it should suggest copying the `uvx` tool as well:
```
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
```

---

_Comment by @zanieb on 2024-10-06 14:28_

👍 yep

---

_Label `documentation` added by @zanieb on 2024-10-06 14:28_

---

_Label `good first issue` added by @zanieb on 2024-10-06 14:28_

---

_Comment by @zaira-bibi on 2024-10-11 10:02_

Hey! I'd like to work on this issue please.

---

_Comment by @Aditya-PS-05 on 2024-10-12 13:35_

hey @zanieb , 
Can you assign me this issue, if nobody is assigned already.

---

_Comment by @zanieb on 2024-10-12 14:28_

This is small enough that we don't need to assign someone to prevent duplicate work — just put up a pull request.

---

_Referenced in [astral-sh/uv#8179](../../astral-sh/uv/pulls/8179.md) on 2024-10-14 17:27_

---

_Comment by @Aditya-PS-05 on 2024-10-14 17:29_

@zanieb , 
please review it.

---

_Closed by @zanieb on 2024-10-14 18:23_

---
