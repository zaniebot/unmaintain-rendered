---
number: 2465
title: Add unit test coverage for netrc authentication
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - testing
  - registry
assignees: []
created_at: 2024-03-14T18:45:29Z
updated_at: 2024-04-17T19:05:25Z
url: https://github.com/astral-sh/uv/issues/2465
synced_at: 2026-01-07T13:12:17-06:00
---

# Add unit test coverage for netrc authentication

---

_Issue opened by @zanieb on 2024-03-14 18:45_

Similar to #2447 — but we should use a netrc file instead of in-url authentication.

We'll probably need to configure netrc to use a file in a temporary directory. We don't want this to affect the developer's machine.

---

_Label `testing` added by @zanieb on 2024-03-14 18:45_

---

_Label `help wanted` added by @zanieb on 2024-03-14 18:45_

---

_Label `registry` added by @zanieb on 2024-03-14 18:45_

---

_Comment by @bschoenmaeckers on 2024-03-14 22:23_

Is [this](https://github.com/astral-sh/uv/blob/main/crates/uv-client/tests/netrc_auth.rs) test not sufficient to validate netrc authentication? Or do we need more tests to validate correct precedence?

---

_Comment by @zanieb on 2024-03-14 23:12_

It's not sufficient because it does doesn't ensure we're properly applying authentication to all the requests necessary for installation. I'm looking for something that's end-to-end like https://github.com/astral-sh/uv/pull/2463

---

_Referenced in [astral-sh/uv#2984](../../astral-sh/uv/pulls/2984.md) on 2024-04-12 19:48_

---

_Referenced in [astral-sh/uv#2976](../../astral-sh/uv/pulls/2976.md) on 2024-04-15 19:55_

---

_Comment by @zanieb on 2024-04-16 16:50_

We have more test coverage in https://github.com/astral-sh/uv/pull/2976 now but still not end-to-end.

---

_Referenced in [astral-sh/uv#3068](../../astral-sh/uv/pulls/3068.md) on 2024-04-16 17:47_

---

_Closed by @zanieb on 2024-04-17 19:05_

---
