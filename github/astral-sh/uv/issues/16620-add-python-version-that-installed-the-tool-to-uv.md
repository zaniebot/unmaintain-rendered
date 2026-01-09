---
number: 16620
title: Add python version that installed the tool to uv tool list
type: issue
state: closed
author: haarisr
labels:
  - enhancement
assignees: []
created_at: 2025-11-06T19:13:18Z
updated_at: 2025-11-06T22:19:07Z
url: https://github.com/astral-sh/uv/issues/16620
synced_at: 2026-01-07T13:12:19-06:00
---

# Add python version that installed the tool to uv tool list

---

_Issue opened by @haarisr on 2025-11-06 19:13_

### Summary

In the current state of uv, uv tool list shows the version of the tool that was installed , but I do not know which python was used to install that tool

Having the python version displayed along with the tool name and version would be helpful.

The issue I ran into was sys.path of a file called with the tool was using the python version that installed the tool (because I didnt specify it) and uv resolved it to using the highest python version maintained by it rather than using the system python.

### Example

_No response_

---

_Label `enhancement` added by @haarisr on 2025-11-06 19:13_

---

_Comment by @my1e5 on 2025-11-06 22:17_

Does `uv tool list --show-python` solve your problem?

---

_Comment by @haarisr on 2025-11-06 22:18_

Yes, should have updated uv first. Thanks!

---

_Closed by @haarisr on 2025-11-06 22:18_

---
