---
number: 8404
title: "`init_app_build_backend_maturin` test timed out on macOS"
type: issue
state: closed
author: zanieb
labels:
  - internal
assignees: []
created_at: 2024-10-21T03:20:35Z
updated_at: 2024-11-26T15:01:05Z
url: https://github.com/astral-sh/uv/issues/8404
synced_at: 2026-01-07T13:12:17-06:00
---

# `init_app_build_backend_maturin` test timed out on macOS

---

_Issue opened by @zanieb on 2024-10-21 03:20_

```
--- STDERR:              uv::it init::init_app_build_backend_maturin ---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Initialized project `foo`

────────────────────────────────────────────────────────────────────────────────


     TIMEOUT [  90.027s] uv::it init::init_lib_build_backend_maturin
```

---

_Label `internal` added by @zanieb on 2024-10-21 03:20_

---

_Renamed from "`init_app_build_backend_maturin` timed out on macOS" to "`init_app_build_backend_maturin` test timed out on macOS" by @zanieb on 2024-10-21 03:20_

---

_Comment by @konstin on 2024-10-21 08:49_

Can you see where it got stuck?

---

_Comment by @zanieb on 2024-10-21 13:37_

Unfortunately no, that's all I got.

---

_Comment by @zanieb on 2024-10-21 22:35_

Here's another https://github.com/astral-sh/uv/actions/runs/11446571344/job/31855447015 also macOS

---

_Comment by @zanieb on 2024-10-21 22:40_

Wow these tests are just insanely slow to begin with

```
        SLOW [  46.554s] uv::it init::init_app_build_backend_maturin
        SLOW [  45.017s] uv::it init::init_lib_build_backend_maturin
```

I bet it's just a bit slower sometimes and times out. It runs in 7s on my machine — I couldn't reproduce a timeout locally. Regardless, this just seems like an unacceptably slow unit test.



---

_Comment by @konstin on 2024-10-21 22:43_

We can speed them up by switching to cffi (no rust deps), pyo3 is quite slow to compile in an uncached integration test (https://github.com/PyO3/maturin/issues/2272).

---

_Comment by @zanieb on 2024-10-21 22:50_

I'm not sure how to do that, but that makes sense to me.

---

_Referenced in [astral-sh/uv#8447](../../astral-sh/uv/pulls/8447.md) on 2024-10-22 12:10_

---

_Closed by @konstin on 2024-10-22 12:17_

---

_Comment by @davidhewitt on 2024-11-26 14:58_

Was part of the problem that the tests were building release binaries? I wonder if setting `MATURIN_PEP517_ARGS=--profile=dev` might have helped if so.

---

_Comment by @konstin on 2024-11-26 15:01_

At least in the maturin test suite itself, even building with dev is slow, and the cache artifacts would be too large to store (https://github.com/PyO3/maturin/issues/2272)

---
