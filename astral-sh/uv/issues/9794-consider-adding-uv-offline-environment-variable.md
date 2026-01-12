```yaml
number: 9794
title: consider adding UV_OFFLINE environment variable
type: issue
state: closed
author: cataggar
labels:
  - configuration
assignees: []
created_at: 2024-12-11T00:36:01Z
updated_at: 2024-12-16T19:46:41Z
url: https://github.com/astral-sh/uv/issues/9794
synced_at: 2026-01-12T15:59:59Z
```

# consider adding UV_OFFLINE environment variable

---

_@cataggar_

I started using `uv` a few days ago. Amazing tool! I want to run a few commands in CI where the network is off. I can add `--offline` to the various commands, but a single `UV_OFFLINE` environment variable would make it easier. Just though I would add an issue for consideration. Tons of commands support `--offline`.



---

_Comment by @zanieb on 2024-12-11 00:47_

Seems reasonable to me, see https://github.com/astral-sh/uv/pull/9795

---

_Label `configuration` added by @zanieb on 2024-12-11 00:47_

---

_Closed by @zanieb on 2024-12-11 15:33_

---

_Closed by @zanieb on 2024-12-11 15:33_

---

_Comment by @max-wittig on 2024-12-16 08:39_

What is the different to `UV_NO_SYNC=1`?

What other online actions exist?

---

_Comment by @zanieb on 2024-12-16 19:46_

`uv add`, `uv pip install`, etc. are all "online" actions

---
