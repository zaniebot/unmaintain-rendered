---
number: 12231
title: uv sync consistently returns error  in combination with local repository
type: issue
state: closed
author: MaartendeRuyter
labels:
  - needs-mre
assignees: []
created_at: 2025-03-17T08:33:48Z
updated_at: 2025-07-16T21:21:01Z
url: https://github.com/astral-sh/uv/issues/12231
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync consistently returns error  in combination with local repository

---

_Issue opened by @MaartendeRuyter on 2025-03-17 08:33_

### Summary

When running uv -sync --upgrade in combination with a local repository, broken pipe errors keep on popping up.
After rerunning uv -sync --upgrade a number of times eventually the sync finishes successfully. See below the terminal output

**Pyproject.toml**
[[tool.uv.index]]
url = "http://localhost:8080/"
default = false

**Terminal Output**
uv sync --upgrade
⠙ bunnet==1.3.0                                                                                                    error: Failed to fetch: `http://localhost:8080/lazy-loader/`
  Caused by: error sending request for url (http://localhost:8080/simple/lazy-loader/)
  Caused by: client error (SendRequest)
  Caused by: error writing a body to connection
  Caused by: Broken pipe (os error 32)
❯ uv sync --upgrade
⠙ location-loader==1.1.5                                                                                           error: Failed to fetch: `http://localhost:8080/mongoengine/`
  Caused by: error sending request for url (http://localhost:8080/simple/mongoengine/)
  Caused by: client error (SendRequest)
  Caused by: error writing a body to connection
  Caused by: Broken pipe (os error 32)
❯ uv sync --upgrade
⠸ python-dotenv==1.0.1                                                                                             
Resolved 98 packages in 2.79s
Prepared 2 packages in 66ms
Uninstalled 2 packages in 14ms
Installed 2 packages in 23ms
 - anyio==4.8.0
 + anyio==4.9.0
 - geo-datagarden-models==1.1.7
 + geo-datagarden-models==1.1.8

### Platform

mac OS15 M2

### Version

uv 0.6.6

### Python version

3.13

---

_Label `bug` added by @MaartendeRuyter on 2025-03-17 08:33_

---

_Comment by @charliermarsh on 2025-03-17 13:22_

I think we'd need a more complete reproduction to help here, like a GitHub repo we can clone and a series of steps we can run to reproduce the error ourselves.

---

_Label `bug` removed by @charliermarsh on 2025-03-17 13:22_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-17 13:22_

---

_Comment by @MaartendeRuyter on 2025-03-17 14:11_

Ok I will try and make a repo where this can be reproduced

---

_Comment by @MaartendeRuyter on 2025-07-16 21:20_

After an update of uv this problem no longer occurs

---

_Closed by @MaartendeRuyter on 2025-07-16 21:21_

---
