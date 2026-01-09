---
number: 8213
title: Support cpython 3.7.9 on linux
type: issue
state: closed
author: timendum
labels: []
assignees: []
created_at: 2024-10-15T12:56:24Z
updated_at: 2026-01-06T08:00:07Z
url: https://github.com/astral-sh/uv/issues/8213
synced_at: 2026-01-07T13:12:17-06:00
---

# Support cpython 3.7.9 on linux

---

_Issue opened by @timendum on 2024-10-15 12:56_

Commit 5f33915e03faffbad46aa0720410dca3f76dcc03 removed the download configuration for CPython 3.7 on linux.

I think it was by accident, the current `uv` version should support python from 3.7.

This issue blocks my tests on 3.7 via Github Actions, it returns:

    Searching for Python versions matching: Python 3.7
    error: No download found for request: cpython-3.7

---

_Referenced in [astral-sh/uv#8216](../../astral-sh/uv/pulls/8216.md) on 2024-10-15 13:29_

---

_Closed by @zanieb on 2024-10-15 16:08_

---

_Comment by @bergentroll on 2025-09-19 07:36_

Still `no download` for `cpython-3.7-linux-x86_64-gnu` on `v0.8.18`.

---

_Comment by @NyaMisty on 2026-01-01 20:04_

Still no download for cpython-3.7-linux-x86_64-gnu on v0.9.21

---

_Comment by @konstin on 2026-01-05 18:28_

Python 3.7 has been end-of-life for 2.5 years now with no bugfix and no security updates (https://devguide.python.org/versions/). We won't add support for this version anymore.

---

_Comment by @bergentroll on 2026-01-06 08:00_

@konstin, nevertheless it us EoL, some people have to support code for it.

---
