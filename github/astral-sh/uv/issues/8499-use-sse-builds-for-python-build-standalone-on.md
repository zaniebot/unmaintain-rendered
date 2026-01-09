---
number: 8499
title: Use SSE builds for Python Build Standalone on linux
type: issue
state: open
author: konstin
labels:
  - enhancement
  - uv python
assignees: []
created_at: 2024-10-23T12:44:06Z
updated_at: 2025-08-13T21:59:50Z
url: https://github.com/astral-sh/uv/issues/8499
synced_at: 2026-01-07T13:12:17-06:00
---

# Use SSE builds for Python Build Standalone on linux

---

_Issue opened by @konstin on 2024-10-23 12:44_

Currently, we're using `x86_64-unknown-linux-gnu-install_only_stripped` build for linux, but there are builds for `x86_64-v2`, `x86_64-v3` and `x86_64-v4` that use SSE to speed up Python (https://gregoryszorc.com/docs/python-build-standalone/main/running.html). We should switch to at least `x86_64_v2` (SSE4), maybe even `x86_64_v3` (AVX2). `x86_64_v4` use AVX-512 which is not available on end user machines, only on server xeons.

---

_Label `enhancement` added by @charliermarsh on 2024-10-23 12:54_

---

_Comment by @zanieb on 2024-10-23 13:21_

I could have sworn there was a request for this already, but don't know where. Regardless, yes we should support these.

---

_Label `uv python` added by @zanieb on 2024-10-23 13:21_

---

_Referenced in [astral-sh/uv#8517](../../astral-sh/uv/pulls/8517.md) on 2024-10-24 06:50_

---

_Comment by @notatallshaw on 2024-10-24 14:18_

> use AVX-512 which is not available on end user machines, only on server xeons.

All new AMD CPUs support AVX-512, both server and consumer: https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#AVX-512_CPU_compatibility_table

Intel consumer CPUs are more complex, some older consumer CPUs do support AVX-512, but newer ones don't.

---

_Comment by @rockisch on 2025-08-13 21:57_

Just making sure, are there still plans to do this change?

My understanding is that as per https://github.com/astral-sh/uv/pull/9781 one should be able to do:
```sh
uv python install cpython-3.12.8-linux-x86_64_v3-gnu
```
to install a version that supports newer instruction sets, but I'm curious if the default will ever be changed.

---

_Comment by @zanieb on 2025-08-13 21:59_

Yeah I have a draft at https://github.com/astral-sh/uv/pull/9788

We're just doing a bit more research on whether these actually boost real world performance and/or if we can do runtime detection within CPython instead.

---
