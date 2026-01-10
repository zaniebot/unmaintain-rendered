---
number: 5684
title: HTTP client build error on Windows
type: issue
state: closed
author: EricThomson
labels:
  - windows
  - needs-mre
assignees: []
created_at: 2024-08-01T13:06:55Z
updated_at: 2024-08-05T15:42:21Z
url: https://github.com/astral-sh/uv/issues/5684
synced_at: 2026-01-10T01:23:51Z
---

# HTTP client build error on Windows

---

_Issue opened by @EricThomson on 2024-08-01 13:06_

`uv` is great thanks for the amazing package! Sorry if this is a duplicate I couldn't find it in the issue queue. 

I'm on Windows 10, working in conda cli, Python 3.10.

uv version:  0.2.32 (38c232e46 2024-07-30)
Command used: `uv pip install tensorboard ` (but I get the same error with any package). 

Error:
> thread 'main' panicked at crates\uv-client\src\base_client.rs:178:33:
> Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 3, kind: NotFound, message: "The system cannot find the path specified." } }

When I downgraded to `0.1.31` installations proceeded without error. 

Stack backtrace didn't seem useful, but here are some lines

>    0:     0x7ff7a631025d - <unknown>
>    1:     0x7ff7a6338019 - <unknown>
...
>   25:     0x7ff7a63e4a0c - <unknown>
>   26:     0x7ffcbc117374 - BaseThreadInitThunk
>   27:     0x7ffcbca1cc91 - RtlUserThreadStart

**Edit** I'm now getting the same error with version `0.2.31`. 🤷‍♂️ So I'm not sure *what* is going on. It doesn't seem `0.2.32` specific.  The installs work fine with `pip install x`. 




---

_Renamed from "HTTP client build error in version 0.2.32 on Windows" to "HTTP client build error on Windows" by @EricThomson on 2024-08-01 13:11_

---

_Comment by @EricThomson on 2024-08-01 13:32_

Note: I created a new virtual environment and things are working fine, so it may just be a problem in the virtual environment I was using. 🤷‍♂️ It doesn't seem to be a systemic problem. 

---

_Comment by @charliermarsh on 2024-08-01 13:34_

Very strange, I wonder what that could be 🤔 

---

_Label `windows` added by @charliermarsh on 2024-08-01 13:34_

---

_Label `needs-mre` added by @charliermarsh on 2024-08-01 13:34_

---

_Comment by @EricThomson on 2024-08-01 13:36_

I'll see if I can reproduce easily, and if not I'll close.

---

_Comment by @samypr100 on 2024-08-02 01:31_

> Note: I created a new virtual environment and things are working fine

Was the old venv pointing at a no longer existing python installation?

---

_Comment by @EricThomson on 2024-08-02 01:49_

> > Note: I created a new virtual environment and things are working fine
> 
> Was the old venv pointing at a no longer existing python installation?

It was an existing Python install, but a fubar ML env that I had piled up too much crap into. 😆 I'll try to recreate in a rational way, but my guess is it will be very hard and I'll end up closing this.

---

_Comment by @EricThomson on 2024-08-05 15:42_

I couldn't reproduce problem, I think I had just done one too many installs of packages in unrecommended ways (and probably had mixed pip, conda, and uv -- I was playing with fire in this environment). Things seem to be fine when I just stick with uv consistently as much as possible. Closing issue. 

---

_Closed by @EricThomson on 2024-08-05 15:42_

---
