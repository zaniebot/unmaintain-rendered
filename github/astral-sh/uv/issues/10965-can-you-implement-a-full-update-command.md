---
number: 10965
title: can you implement a full update command
type: issue
state: closed
author: oovaa
labels:
  - enhancement
assignees: []
created_at: 2025-01-26T06:48:47Z
updated_at: 2025-01-26T16:27:42Z
url: https://github.com/astral-sh/uv/issues/10965
synced_at: 2026-01-07T13:12:18-06:00
---

# can you implement a full update command

---

_Issue opened by @oovaa on 2025-01-26 06:48_

### Summary

for now im using this script

uv pip list --outdated | tail -n +2 | tr -s ' ' | cut -d ' ' -f 1 | tr '\n' ' ' | xargs -n 1 uv pip install --upgrade

### Example

_No response_

---

_Label `enhancement` added by @oovaa on 2025-01-26 06:48_

---

_Closed by @zanieb on 2025-01-26 16:27_

---

_Comment by @zanieb on 2025-01-26 16:27_

We're tracking this over in https://github.com/astral-sh/uv/issues/1419

---
