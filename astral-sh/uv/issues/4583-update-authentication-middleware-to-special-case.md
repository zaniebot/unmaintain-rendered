---
number: 4583
title: Update authentication middleware to special-case index urls
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - network
assignees: []
created_at: 2024-06-27T11:03:00Z
updated_at: 2025-04-29T21:37:04Z
url: https://github.com/astral-sh/uv/issues/4583
synced_at: 2026-01-10T01:23:39Z
---

# Update authentication middleware to special-case index urls

---

_Issue opened by @zanieb on 2024-06-27 11:03_

As discussed in https://github.com/astral-sh/uv/issues/4056, pip makes a keyring request for credentials for the _index url_ but we make a request for the _package url_ and the _index url host name_ which for some plugins (e.g. Azure #3542) does not seem to work.

We should update the authentication handling to be aware of the user-provided index urls and, when we see a url that is a child of an index url, attempt to fetch credentials for the index url either after or instead of the package url.

---

_Label `help wanted` added by @zanieb on 2024-06-27 11:03_

---

_Label `network` added by @zanieb on 2024-06-27 11:03_

---

_Referenced in [astral-sh/uv#3542](../../astral-sh/uv/issues/3542.md) on 2024-07-04 21:28_

---

_Referenced in [astral-sh/uv#4857](../../astral-sh/uv/pulls/4857.md) on 2024-07-08 09:18_

---

_Referenced in [astral-sh/uv#5367](../../astral-sh/uv/pulls/5367.md) on 2024-07-23 20:28_

---

_Referenced in [astral-sh/uv#5600](../../astral-sh/uv/issues/5600.md) on 2024-07-30 16:32_

---

_Referenced in [astral-sh/uv#3923](../../astral-sh/uv/issues/3923.md) on 2024-08-22 17:09_

---

_Referenced in [astral-sh/uv#6389](../../astral-sh/uv/pulls/6389.md) on 2024-08-22 18:23_

---

_Referenced in [astral-sh/uv#7545](../../astral-sh/uv/pulls/7545.md) on 2024-09-21 14:01_

---

_Referenced in [astral-sh/uv#8256](../../astral-sh/uv/pulls/8256.md) on 2024-10-16 14:20_

---

_Referenced in [astral-sh/uv#8565](../../astral-sh/uv/issues/8565.md) on 2024-10-25 14:20_

---

_Referenced in [astral-sh/uv#10594](../../astral-sh/uv/issues/10594.md) on 2025-01-14 19:04_

---

_Referenced in [astral-sh/uv#11017](../../astral-sh/uv/issues/11017.md) on 2025-01-29 15:00_

---

_Referenced in [astral-sh/uv#11074](../../astral-sh/uv/pulls/11074.md) on 2025-01-29 17:53_

---

_Referenced in [astral-sh/uv#11236](../../astral-sh/uv/issues/11236.md) on 2025-02-06 16:57_

---

_Referenced in [astral-sh/uv#11391](../../astral-sh/uv/issues/11391.md) on 2025-02-10 17:01_

---

_Referenced in [astral-sh/uv#11507](../../astral-sh/uv/issues/11507.md) on 2025-02-14 13:02_

---

_Referenced in [astral-sh/uv#4056](../../astral-sh/uv/issues/4056.md) on 2025-03-20 22:03_

---

_Referenced in [astral-sh/uv#12004](../../astral-sh/uv/issues/12004.md) on 2025-03-20 22:21_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-02 17:33_

---

_Referenced in [astral-sh/uv#12651](../../astral-sh/uv/pulls/12651.md) on 2025-04-03 14:05_

---

_Closed by @zanieb on 2025-04-29 21:37_

---
