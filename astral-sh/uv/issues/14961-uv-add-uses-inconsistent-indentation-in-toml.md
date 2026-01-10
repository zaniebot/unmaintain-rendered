---
number: 14961
title: "`uv add` uses inconsistent indentation in toml after conversion to multiline list"
type: issue
state: closed
author: KRRT7
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-07-29T21:21:41Z
updated_at: 2025-07-31T11:50:06Z
url: https://github.com/astral-sh/uv/issues/14961
synced_at: 2026-01-10T01:25:51Z
---

# `uv add` uses inconsistent indentation in toml after conversion to multiline list

---

_Issue opened by @KRRT7 on 2025-07-29 21:21_

### Summary

<img width="766" height="362" alt="Image" src="https://github.com/user-attachments/assets/6e3299ab-8971-42d4-8840-894f70975891" />


clone https://github.com/pydantic/pydantic-ai 
add codeflash to lint group `uv add --group lint codeflash`

### Platform

macOS

### Version

0.8.3

### Python version

3.12.8

---

_Label `bug` added by @KRRT7 on 2025-07-29 21:21_

---

_Renamed from "formatting in pyproject is off after adding to lint group" to "`uv add` uses inconsistent indentation in toml after conversion to multiline list" by @zanieb on 2025-07-29 21:59_

---

_Label `help wanted` added by @zanieb on 2025-07-29 21:59_

---

_Comment by @zanieb on 2025-07-29 21:59_

Thanks!

---

_Comment by @charliermarsh on 2025-07-31 02:06_

I sort of wonder if this is related to the `toml-edit` upgrade? But I assume we had test coverage for this.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-31 02:47_

---

_Referenced in [astral-sh/uv#14991](../../astral-sh/uv/pulls/14991.md) on 2025-07-31 11:24_

---

_Closed by @zanieb on 2025-07-31 11:50_

---
