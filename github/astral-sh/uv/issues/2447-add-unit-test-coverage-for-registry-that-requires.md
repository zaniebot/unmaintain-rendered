---
number: 2447
title: Add unit test coverage for registry that requires basic HTTP authentication
type: issue
state: closed
author: zanieb
labels:
  - testing
  - registry
assignees: []
created_at: 2024-03-14T03:36:32Z
updated_at: 2024-03-15T14:50:05Z
url: https://github.com/astral-sh/uv/issues/2447
synced_at: 2026-01-07T13:12:17-06:00
---

# Add unit test coverage for registry that requires basic HTTP authentication

---

_Issue opened by @zanieb on 2024-03-14 03:36_

We're missing coverage for this and it's fairly critical.

We'll need to host an index somewhere that we can expose credentials to.

e.g. to prevent regressions like https://github.com/astral-sh/uv/issues/2444

---

_Label `testing` added by @zanieb on 2024-03-14 03:36_

---

_Label `registry` added by @zanieb on 2024-03-14 03:45_

---

_Assigned to @zanieb by @zanieb on 2024-03-14 15:30_

---

_Comment by @zanieb on 2024-03-14 18:09_

A NGINX proxy with HTTP basic authentication in progress at https://github.com/astral-sh/pypi-proxy/pull/1

---

_Referenced in [astral-sh/uv#2463](../../astral-sh/uv/pulls/2463.md) on 2024-03-14 18:18_

---

_Referenced in [astral-sh/uv#2464](../../astral-sh/uv/issues/2464.md) on 2024-03-14 18:44_

---

_Referenced in [astral-sh/uv#2465](../../astral-sh/uv/issues/2465.md) on 2024-03-14 18:45_

---

_Comment by @vlad-ivanov-name on 2024-03-15 14:11_

Not sure if this is useful but a few weeks ago I implemented a "simple" pip server for integration tests in pixi:

https://github.com/vlad-ivanov-name/pixi/blob/f52c9ac1772cb623f257cd222e303480bcf76940/tests/common/pypi_server.rs

pixi has since switched to uv as backend so the associated PR was obsolete and was closed, but perhaps the code could be useful here. It creates a local server with Axum and creates wheels on the fly -- all from rust, without needing external processes, containers, etc. Authentication would be really easy to add there via axum auth middleware.

---

_Comment by @zanieb on 2024-03-15 14:42_

Awesome thanks! It's on my wishlist to have a local server for the test suite. I'll take a look at that.

---

_Closed by @zanieb on 2024-03-15 14:50_

---
