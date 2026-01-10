---
number: 14010
title: "`uv publish` should continue after rejected wheels"
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2025-06-12T22:41:02Z
updated_at: 2025-09-26T13:09:56Z
url: https://github.com/astral-sh/uv/issues/14010
synced_at: 2026-01-10T01:25:41Z
---

# `uv publish` should continue after rejected wheels

---

_Issue opened by @zanieb on 2025-06-12 22:41_

In https://github.com/astral-sh/uv/issues/14007, I discovered that `uv publish` fails eagerly instead of at the end of the publish process. Unfortunately, this leaves a release in a bad state where it is missing a bunch of artifacts instead of just the one rejected distribution. We should upload everything we can, then fail.

---

_Referenced in [astral-sh/uv#16028](../../astral-sh/uv/issues/16028.md) on 2025-09-25 19:38_

---

_Comment by @zanieb on 2025-09-25 19:38_

@konstin what's your stance on this?

---

_Comment by @konstin on 2025-09-26 13:09_

As usual, ideally we had Upload API 2.0 which solves this cleanly. It seems reasonable to continue after errors if there was already a previous upload that had succeeded.

---
