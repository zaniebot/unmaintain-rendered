---
number: 13097
title: env export and import with tarball
type: issue
state: closed
author: coconil
labels:
  - enhancement
assignees: []
created_at: 2025-04-25T03:43:50Z
updated_at: 2025-04-25T07:56:24Z
url: https://github.com/astral-sh/uv/issues/13097
synced_at: 2026-01-07T13:12:18-06:00
---

# env export and import with tarball

---

_Issue opened by @coconil on 2025-04-25 03:43_

### Summary

I need something like `conda pack` to support offline workflow.

the server to deploy my project have no internet ,so it is imposible to download package from PypI. 
when using conda,we can pack a conda env to my_env.tar.gz and then send it to server ,then source it.

it seems that uv is lack of method to work without internet.



### Example

_No response_

---

_Label `enhancement` added by @coconil on 2025-04-25 03:43_

---

_Comment by @konstin on 2025-04-25 07:56_

Duplicate of #7865, #6970

---

_Closed by @konstin on 2025-04-25 07:56_

---
