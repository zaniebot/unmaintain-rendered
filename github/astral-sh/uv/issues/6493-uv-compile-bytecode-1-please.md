---
number: 6493
title: UV_COMPILE_BYTECODE=1 please? 🥺
type: issue
state: closed
author: hynek
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2024-08-23T05:12:27Z
updated_at: 2024-08-23T18:05:34Z
url: https://github.com/astral-sh/uv/issues/6493
synced_at: 2026-01-07T13:12:17-06:00
---

# UV_COMPILE_BYTECODE=1 please? 🥺

---

_Issue opened by @hynek on 2024-08-23 05:12_

Hello, it is I again, your obnoxious Docker user!

As the documentation correctly states, bytecode-compilation is super useful in Docker containers. Could we have a `UV_COMPILE_BYTECODE=1` to set once and forget?

If one only uses it to install one app, it's no big deal to also pass `--compile-bytecode`, but thanks to `uv tool`, I've got the following block in my build container:

```dockerfile
ENV PATH=/root/.local/bin:/root/.cargo/bin:$PATH \
    UV_INDEX_URL=$PIP_INDEX_URL \
    UV_LINK_MODE=copy \
    UV_PYTHON_DOWNLOADS=never \
    UV_PYTHON=/usr/bin/python3.12

RUN --mount=type=cache,target=/root/.cache \
    set -ex \
    && uv tool install --compile-bytecode secret-internal-tool \
    && uv tool install --compile-bytecode pdm \
    && uv tool install --compile-bytecode hatch \
    && uv tool install --compile-bytecode virtualenv \
    && ...
```

It would be great if I could just move it to the ENVs.

---

_Label `enhancement` added by @konstin on 2024-08-23 07:39_

---

_Label `good first issue` added by @zanieb on 2024-08-23 13:13_

---

_Label `configuration` added by @zanieb on 2024-08-23 13:13_

---

_Label `enhancement` removed by @zanieb on 2024-08-23 13:13_

---

_Comment by @charliermarsh on 2024-08-23 15:49_

Will do this real quick between meetings.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 15:52_

---

_Referenced in [astral-sh/uv#6530](../../astral-sh/uv/pulls/6530.md) on 2024-08-23 15:52_

---

_Closed by @charliermarsh on 2024-08-23 18:05_

---

_Closed by @charliermarsh on 2024-08-23 18:05_

---
