---
number: 8223
title: Provide hints when trusted publishing fails
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-10-15T16:15:12Z
updated_at: 2024-10-28T20:13:44Z
url: https://github.com/astral-sh/uv/issues/8223
synced_at: 2026-01-07T13:12:17-06:00
---

# Provide hints when trusted publishing fails

---

_Issue opened by @konstin on 2024-10-15 16:15_

When trusted publishing on github actions was requested but fails, it's usually due to some mismatch between pypi's configuration and the actual job, e.g., a missing `.yml` extension. It's cumbersome to debug since pypi will not tell you what mismatched for security, and you can only use trusted publishing in github actions, so each debugging round is starting a new publish job. To help with trusted publishing errors, we should provide a printout with debug information that the user can easily diff against the fields they entered in the pypi web interface and their github actions job configuration.

---

_Label `enhancement` added by @konstin on 2024-10-15 16:15_

---

_Referenced in [astral-sh/uv#8632](../../astral-sh/uv/pulls/8632.md) on 2024-10-28 11:04_

---

_Referenced in [astral-sh/uv#8633](../../astral-sh/uv/pulls/8633.md) on 2024-10-28 12:49_

---

_Closed by @konstin on 2024-10-28 20:13_

---
