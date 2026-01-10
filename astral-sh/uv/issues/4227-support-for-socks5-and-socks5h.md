---
number: 4227
title: Support for SOCKS5 and SOCKS5H
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - network
assignees: []
created_at: 2024-06-11T01:52:30Z
updated_at: 2024-09-18T15:59:38Z
url: https://github.com/astral-sh/uv/issues/4227
synced_at: 2026-01-10T01:23:35Z
---

# Support for SOCKS5 and SOCKS5H

---

_Issue opened by @zanieb on 2024-06-11 01:52_

We don't support these but it looks like `reqwest` does so we could.

Prompted in https://github.com/astral-sh/uv/issues/4186

---

_Label `enhancement` added by @zanieb on 2024-06-11 01:52_

---

_Label `network` added by @zanieb on 2024-06-11 01:52_

---

_Referenced in [astral-sh/uv#4186](../../astral-sh/uv/issues/4186.md) on 2024-06-11 01:53_

---

_Comment by @hongyi-zhao on 2024-06-11 02:23_

Do you mean `SOCKSSH` is an alias of `SOCKS5H`, aka, [SOCKS5-hostname](https://everything.curl.dev/usingcurl/proxies/socks.html) used by curl?

See https://chengke.name/socks5-proxy-scheme-with-remote-dns-resolving/ for the related discussion.

---

_Comment by @zanieb on 2024-06-11 04:11_

The latter, thanks! 🤦‍♀️ 

---

_Renamed from "Support for SOCKS5 and SOCKSSH" to "Support for SOCKS5 and SOCKS5H" by @zanieb on 2024-06-11 04:11_

---

_Referenced in [astral-sh/uv#7503](../../astral-sh/uv/pulls/7503.md) on 2024-09-18 15:20_

---

_Comment by @zanieb on 2024-09-18 15:59_

Added in #7503

---

_Closed by @zanieb on 2024-09-18 15:59_

---
