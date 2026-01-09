---
number: 274
title: "Worker: Uncaught TypeError: This WritableStream has been closed."
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-01T15:16:50Z
updated_at: 2023-11-30T01:57:10Z
url: https://github.com/astral-sh/uv/issues/274
synced_at: 2026-01-07T13:12:16-06:00
---

# Worker: Uncaught TypeError: This WritableStream has been closed.

---

_Issue opened by @konstin on 2023-11-01 15:16_

The worker prints a lot of errors in the form of
```
3:54:01 PM GET /packages/07/6c/aa3f2f849e01cb6a001cd8554a88d4c77c5c1a31c95bdf1cf9301e6d9ef4/defusedxml-0.7.1-py2.py3-none-any.whl 200
Response for request url: https://pypi-metadata.konsti.workers.dev/packages/7e/80/cab10959dc1faead58dc8384a781dfbf93cb4d33d50988f7a69f1b7c9bbe/oauthlib-3.2.2-py3-none-any.whl not present in cache. Fetching and caching request.
✘ [ERROR] Uncaught (async) TypeError: This WritableStream has been closed.


✘ [ERROR] Uncaught (in promise) TypeError: This WritableStream has been closed.
```
even when the requests are successful. These are likely from zip.js.

---

_Label `bug` added by @charliermarsh on 2023-11-01 15:19_

---

_Comment by @charliermarsh on 2023-11-03 00:19_

I can't get these to show up anymore... What did I do?

---

_Comment by @konstin on 2023-11-06 10:47_

Maybe you have everything cached? For me they still show up

---

_Added to milestone `Future` by @charliermarsh on 2023-11-06 21:06_

---

_Removed from milestone `Future` by @konstin on 2023-11-24 09:47_

---

_Comment by @konstin on 2023-11-24 09:47_

Removing this from the milestone since we're not using the worker anymore

---

_Closed by @charliermarsh on 2023-11-30 01:57_

---
