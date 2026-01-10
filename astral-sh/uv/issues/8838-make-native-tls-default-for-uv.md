---
number: 8838
title: "Make `native-tls` default for uv?"
type: issue
state: open
author: notatallshaw-gts
labels:
  - needs-decision
assignees: []
created_at: 2024-11-05T19:12:36Z
updated_at: 2025-10-30T18:01:09Z
url: https://github.com/astral-sh/uv/issues/8838
synced_at: 2026-01-10T01:24:33Z
---

# Make `native-tls` default for uv?

---

_Issue opened by @notatallshaw-gts on 2024-11-05 19:12_

Pip 24.2+, on Python 3.10+, you get truststore by default: https://pip.pypa.io/en/stable/topics/https-certificates/#using-system-certificate-stores / https://github.com/sethmlarson/truststore

As I understand it, this is roughly equivalent to uv's `native-tls`. Would it make sense for uv to switch `native-tls` on by default and have an option to turn off if needed?

---

_Comment by @zanieb on 2024-11-05 19:27_

See also 

- https://github.com/encode/httpx/issues/302
- https://github.com/astral-sh/uv/pull/2362
- https://github.com/rustls/rustls-platform-verifier

Notably in https://github.com/astral-sh/uv/pull/2362 we identified that enabling native tls by default was weirdly slow on macOS.


---

_Label `needs-decision` added by @zanieb on 2024-11-05 20:20_

---

_Comment by @notatallshaw on 2025-09-25 13:40_

Would still love to see this, I think it's a big enough improvement in enterprise environments that the default should be true for non-macOS platforms if that's still an issue.

---

_Comment by @michael-o on 2025-10-29 22:24_

Same here. All enterprise intermediate and root CAs are in the truststore. It is either native or bust.
I'd like also to point out that cargo uses the native store by default. I guess uv should have the same experience...

---
