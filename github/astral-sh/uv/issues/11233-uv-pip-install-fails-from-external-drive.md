---
number: 11233
title: uv pip install fails from external drive
type: issue
state: closed
author: robinjhuang
labels:
  - bug
assignees: []
created_at: 2025-02-05T04:42:54Z
updated_at: 2025-02-13T23:11:42Z
url: https://github.com/astral-sh/uv/issues/11233
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip install fails from external drive

---

_Issue opened by @robinjhuang on 2025-02-05 04:42_

uv: 0.5.26

While trying to install torch on an external harddrive, I get this error: 

```
[2025-02-04 19:49:50.325] [info]    × Failed to download `typing-extensions==4.12.2`
  ├─:arrow_forward: Failed to read from the distribution cache
  ╰─:arrow_forward: Incorrect function. (os error 1)
  help: `typing-extensions` (v4.12.2) was included because `torch`
        (v2.6.0+cu124) depends on `typing-extensions`
PS D:\> echo "_-end-1738727383800:$?"
```

I noticed this only happens when the drive is exFat format. NTFS works fine. Also I set these environment variables to also live on the external drive: 

```
UV_CACHE_DIR
UV_TOOL_DIR
UV_TOOL_BIN_DIR
UV_PYTHON_INSTALL_DIR

```

I tried setting the cache on the main file system and I get a warning that the file systems are differentm, but it still works ultimately. 

![Image](https://github.com/user-attachments/assets/3207c4f4-a220-4680-b0b9-1d15cad79d71)

---

_Comment by @zanieb on 2025-02-05 04:49_

So both `UV_CACHE_DIR` and the install target are on an exFAT drive? Is it fixed by setting `UV_LINK_MODE=copy`?

---

_Comment by @robinjhuang on 2025-02-05 20:34_

Yeah, both cache dir and the install target are on the drive. 
I tried `UV_LINK_MODE=copy` and `symlink` and both give the same error. I wonder if it's because uv is not on the external drive? The main drive is on a different file system.

---

_Comment by @zanieb on 2025-02-05 20:41_

It shouldn't matter that we're on different drives, afaik.

Interesting that it fails on _read_

> Failed to read from the distribution cache

Can you share debug logs as described in #9452? Is there a reason you bypassed our issue template?

---

_Label `bug` added by @zanieb on 2025-02-05 20:42_

---

_Comment by @robinjhuang on 2025-02-13 23:11_

Apologies! Will use the template next time!

---

_Closed by @robinjhuang on 2025-02-13 23:11_

---
