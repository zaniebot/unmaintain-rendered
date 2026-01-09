---
number: 11158
title: "`ruff server` should check `find_user_settings_toml()` if no file configuration exists"
type: issue
state: closed
author: snowsignal
labels:
  - configuration
  - server
assignees: []
created_at: 2024-04-26T07:54:27Z
updated_at: 2024-05-01T09:08:52Z
url: https://github.com/astral-sh/ruff/issues/11158
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server` should check `find_user_settings_toml()` if no file configuration exists

---

_Issue opened by @snowsignal on 2024-04-26 07:54_

When a workspace folder doesn't have any configuration available, `ruff server` should try `find_user_settings_toml()` as a fallback. This would try reading `~/.config/ruff/.ruff.toml`, `~/.config/ruff/ruff.toml` and `~/.config/ruff/pyproject.toml` on Linux.

---

_Label `configuration` added by @snowsignal on 2024-04-26 07:54_

---

_Label `server` added by @snowsignal on 2024-04-26 07:54_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-26 07:54_

---

_Referenced in [astral-sh/ruff#11140](../../astral-sh/ruff/pulls/11140.md) on 2024-04-26 07:55_

---

_Comment by @GaetanLepage on 2024-04-29 12:06_

Has this been fixed within #11140 or it it needs another PR ?

---

_Comment by @snowsignal on 2024-04-29 14:26_

It needs another PR, which should be open soon!

---

_Referenced in [astral-sh/ruff#11225](../../astral-sh/ruff/pulls/11225.md) on 2024-05-01 01:46_

---

_Closed by @snowsignal on 2024-05-01 09:08_

---
