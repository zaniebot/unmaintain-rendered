---
number: 3841
title: Improve error message when discovery fails due to system interpreter filtering
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-05-26T15:03:29Z
updated_at: 2024-05-26T15:03:33Z
url: https://github.com/astral-sh/uv/issues/3841
synced_at: 2026-01-07T13:12:17-06:00
---

# Improve error message when discovery fails due to system interpreter filtering

---

_Issue opened by @zanieb on 2024-05-26 15:03_

e.g. in https://github.com/astral-sh/uv/issues/3837 we say we can't find the interpreter in all sources but actually we filtered out the found interpreters because `--system` wasn't included. We should detect this and hint to the user to use the `--system` flag. The user should not need to turn on verbose logs to see what's happening.

---

_Label `error messages` added by @zanieb on 2024-05-26 15:03_

---

_Referenced in [astral-sh/uv#3861](../../astral-sh/uv/issues/3861.md) on 2024-05-27 09:51_

---
