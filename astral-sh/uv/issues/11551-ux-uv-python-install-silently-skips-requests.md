---
number: 11551
title: "UX: `uv python install` silently skips requests depending on what I already have installed"
type: issue
state: open
author: neutrinoceros
labels:
  - enhancement
  - cli
assignees: []
created_at: 2025-02-16T10:11:15Z
updated_at: 2025-03-12T14:52:25Z
url: https://github.com/astral-sh/uv/issues/11551
synced_at: 2026-01-10T01:25:07Z
---

# UX: `uv python install` silently skips requests depending on what I already have installed

---

_Issue opened by @neutrinoceros on 2025-02-16 10:11_

### Summary

Starting from a state where I don't have a managed copy of Python 3.14 installed. Installing works as expected
```
❯ uv python install 3.14
Installed Python 3.14.0a5 in 15.92s
 + cpython-3.14.0a5-macos-aarch64-none
```
However, immediately running the same command doesn't produce any output
```
❯ uv python install 3.14
```
this is surprising to me: I would expect a "warning" of some sort to tell me that I already have the requested version installed, which is why nothing was done.
Even in verbose mode, there doesn't seem to be a clear indication of that skip
```
❯ uv python install 3.14 -v
DEBUG uv 0.6.0 (591f38c25 2025-02-14)
DEBUG Acquired lock for `/Users/clm/.local/share/uv/python`
DEBUG Found `cpython-3.14.0a5-macos-aarch64-none` for request `Python 3.14`
DEBUG Using request timeout of 30s
DEBUG Skipping installation of Python executables, use `--preview` to enable.
DEBUG Released lock at `/Users/clm/.local/share/uv/python/.lock`
```


Of course in the context of this minimal reprod it is pretty obvious what's happening, but it's a little bit more confusing when running a longer command such as, e.g.
```
❯ uv python install 3.14 3.13 3.12 3.11
```
and all versions I requested *but one* appear on the output.

*update*: this second part was resolved in https://github.com/astral-sh/uv/pull/12124
I'm putting it in quote mode to highlight this fact

>Similarily, if I try to re-install a version I *don't* already have, it is also silently skipped
>```
>❯ uv python install 3.14t --reinstall
>```
>This time, in verbose mode, the version I asked for is not even mentioned
>```
>❯ uv python install 3.14t --reinstall -v
>DEBUG uv 0.6.0 (591f38c25 2025-02-14)
>DEBUG Acquired lock for `/Users/clm/.local/share/uv/python`
>DEBUG Using request timeout of 30s
>DEBUG Released lock at `/Users/clm/.local/share/uv/python/.lock`
>```


### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.0 (591f38c25 2025-02-14)

### Python version

Python 3.13.2

---

_Label `bug` added by @neutrinoceros on 2025-02-16 10:11_

---

_Label `bug` removed by @zanieb on 2025-02-16 15:35_

---

_Label `enhancement` added by @zanieb on 2025-02-16 15:35_

---

_Label `cli` added by @zanieb on 2025-02-16 15:35_

---

_Comment by @zanieb on 2025-02-16 15:36_

Seems like we could do something better here

For reference, the output from, e.g., `uv pip install`, looks like

```
❯ uv pip install anyio
Resolved 3 packages in 77ms
Installed 3 packages in 7ms
 + anyio==4.8.0
 + idna==3.10
 + sniffio==1.3.1
❯ uv pip install anyio
Audited 1 package in 2ms
```

Also the log that your request is satisfied is

> DEBUG Found `cpython-3.14.0a5-macos-aarch64-none` for request `Python 3.14`

---

_Comment by @neutrinoceros on 2025-02-16 16:26_

> Also the log that your request is satisfied is

Ah ! I wasn't sure if this line was about the requested interpreter found on my system or on the host server !

---

_Comment by @zanieb on 2025-02-16 16:34_

Yeah the message for that is, e.g.

> DEBUG Found download `cpython-3.12.8-macos-aarch64-none` for request `Python 3.12.8`

We can clear things up here though

---

_Comment by @neutrinoceros on 2025-03-12 06:22_

#12124 was merged and looks to me like it partially address this issue.
@zanieb do you know if this should be closed ?

---

_Comment by @neutrinoceros on 2025-03-12 13:53_

nevermind I just tried it again, and this part is sill relevant

> this is surprising to me: I would expect a "warning" of some sort to tell me that I already have the requested version installed, which is why nothing was done.

I'll edit the OP to reflect that part of it is now solved though

---

_Comment by @zanieb on 2025-03-12 14:47_

👍 I can work on the messaging still, just lots of tasks on the backlog :)

---

_Comment by @neutrinoceros on 2025-03-12 14:52_

No rush at all. In fact, I'm game to give it a try myself.

---
