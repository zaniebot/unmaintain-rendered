---
number: 12307
title: uv Support mirror？
type: issue
state: closed
author: Dengjun21
labels:
  - question
assignees: []
created_at: 2025-03-19T02:13:29Z
updated_at: 2025-03-19T03:48:31Z
url: https://github.com/astral-sh/uv/issues/12307
synced_at: 2026-01-07T13:12:18-06:00
---

# uv Support mirror？

---

_Issue opened by @Dengjun21 on 2025-03-19 02:13_

### Question

I encounter the `error :  Caused by: operation timed out when i  run uv python install 3.12  .`
And I also want to know It can trasnlate the `uv.lock` to `requriment.txt`?  maybe for dexterity? 
**Thanks**

### Platform

win11

### Version

uv 0.6.8 (c1ef48276 2025-03-18)

---

_Label `question` added by @Dengjun21 on 2025-03-19 02:13_

---

_Comment by @zanieb on 2025-03-19 03:18_

You can accomplish `uv.lock` -> `requirements.txt` with `uv export`.

We'll need a more details about the error to help. Please see #9452  

---

_Comment by @Dengjun21 on 2025-03-19 03:48_

@zanieb Thanks, this could be caused by a busy network, I waited a while and retried and it worked! 

---

_Closed by @Dengjun21 on 2025-03-19 03:48_

---
