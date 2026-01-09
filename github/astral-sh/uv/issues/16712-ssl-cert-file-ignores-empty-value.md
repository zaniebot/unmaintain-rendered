---
number: 16712
title: SSL_CERT_FILE ignores empty value
type: issue
state: open
author: samypr100
labels:
  - good first issue
assignees: []
created_at: 2025-11-12T20:42:52Z
updated_at: 2025-12-16T21:35:26Z
url: https://github.com/astral-sh/uv/issues/16712
synced_at: 2026-01-07T13:12:19-06:00
---

# SSL_CERT_FILE ignores empty value

---

_Issue opened by @samypr100 on 2025-11-12 20:42_

We don't do this for SSL_CERT_FILE, do we want to align there?

_Originally posted by @samypr100 in https://github.com/astral-sh/uv/pull/16473#discussion_r2515864179_

Goal is to use the same filter as https://github.com/astral-sh/uv/blob/63ab24776566bb76d68751e2ee5180b6cec34090/crates/uv/src/settings.rs#L107, but for `SSL_CERT_FILE`.

---

_Referenced in [astral-sh/uv#16473](../../astral-sh/uv/pulls/16473.md) on 2025-11-12 20:45_

---

_Label `good first issue` added by @zanieb on 2025-11-13 13:41_

---

_Comment by @taearls on 2025-11-16 17:11_

If no one is actively working on this, I'd be happy to give it a try!

---

_Comment by @samypr100 on 2025-11-16 18:18_

@taearls feel free 👍 

---

_Referenced in [astral-sh/uv#16772](../../astral-sh/uv/pulls/16772.md) on 2025-11-18 18:19_

---

_Comment by @tysoncung on 2025-12-15 02:24_

I'm interested in working on this issue. Could you provide more context about what you're looking for? Any additional details about requirements or constraints would be helpful.

---

_Comment by @taearls on 2025-12-16 21:35_

> I'm interested in working on this issue. Could you provide more context about what you're looking for? Any additional details about requirements or constraints would be helpful.

Hey! Just as an FYI, there's an open PR that's already linked to this issue. It just needs one additional approval, and then it will likely be merged shortly after.

I would recommend looking for another issue without an open PR linked to it.

---
