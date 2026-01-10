---
number: 10340
title: On ubuntu 24.04LTS OS tkinter app is not working other than system python version install using uv python install
type: issue
state: closed
author: learncodingforweb
labels: []
assignees: []
created_at: 2025-01-07T06:34:46Z
updated_at: 2025-09-08T14:59:34Z
url: https://github.com/astral-sh/uv/issues/10340
synced_at: 2026-01-10T01:24:52Z
---

# On ubuntu 24.04LTS OS tkinter app is not working other than system python version install using uv python install

---

_Issue opened by @learncodingforweb on 2025-01-07 06:34_

```
uname -a
Linux embedded 6.8.0-51-generic #52-Ubuntu SMP PREEMPT_DYNAMIC Thu Dec  5 13:09:44 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

system has python version 
python --version
Python 3.12.3
which python
/usr/bin/python
```
for which tkinter is working fine.

But if i install python other than system version. than uv is not able to find tkinter and getting error
This probably means that Tcl wasn't installed properly.
```
uv --version
uv 0.5.13
```
Even i tried
```
uv python uninstall 3.13
uv python install 3.13
```
I have observed this problem on Ubuntu 24.04LTS?

---

_Comment by @konstin on 2025-01-07 10:14_

This is a duplicate of #6893 / https://github.com/astral-sh/python-build-standalone/issues/129

---

_Closed by @konstin on 2025-01-07 10:14_

---

_Comment by @geofft on 2025-08-08 21:29_

The error "This probably means that Tcl wasn't installed properly." is not the same as 6893/129 - it's #7036 / astral-sh/python-build-standalone#421. It should have been fixed a long time ago. If you're still seeing it, please take a look at the suggestions in the https://github.com/astral-sh/uv/issues/7036#issuecomment-2963558120 .

If that doesn't resolve the issue, can you please report what the exact uv command you're running that gets the error, and what the output of `uv run python -VV` is? I want to make sure you're not still on an old version of Python. `uv python uninstall 3.13; uv python install 3.13` is sometimes not sufficient.

(Perhaps you upgraded in the last few months and this is no longer happening....)

---

_Comment by @EvandroJRSilva on 2025-09-08 14:49_

I'm still facing the same issue. In my case I'm trying to run a cell with `ipykernel` in VS Code. Running `uv run python -VV` I get: 

`Python 3.13.3 (main, Apr  9 2025, 04:03:52) [Clang 20.1.0 ]`

The code I'm trying to run:

```
import tkinter as tk

root = tk.Tk()

message = tk.Label(root, text="Hello, World!")
message.pack()

root.mainloop()
```

What I've tried:

1. `uv python install --reinstall 3.13`, deleted `.venv` and created a new one.
2. `uv python uninstall 3.13` followed by `uv python install 3.13` and creating a new `.venv`.
3. I created a `.py` file with the same code. If I run this file or `python -m tkinter` (with uv venv activated) I receive the following message:

```
[xcb] Unknown sequence number while appending request
[xcb] You called XInitThreads, this is not your fault
[xcb] Aborting, sorry about that.
python: ../../src/xcb_io.c:157: append_pending_request: Assertion `!xcb_xlib_unknown_seq_number' failed.
Abortado (imagem do núcleo gravada)
```
Which seems to me some kind of memory error.

**Note**: if I run the same file under system interpreter, it runs normally.

### THE WEIRDEST OF ALL

If the code is the following:

```
import tkinter as tk

root = tk.Tk()
root.mainloop()
```
It runs normally!

---

_Comment by @zanieb on 2025-09-08 14:53_

You should update to a newer version of uv then reinstall Python, we fixed this in the latest build of 3.13.7.

---

_Comment by @geofft on 2025-09-08 14:53_

@EvandroJRSilva The bug you're describing was fixed recently (it's not the same as the bug this issue was about), and also
it sounds like you're on an older version of uv itself, which is getting you an older build of our Python versions. Can you try

```
uv self update && uv python upgrade --reinstall
```

? (The date in your Python version needs to be at least August 8, 2025.)

---

_Comment by @EvandroJRSilva on 2025-09-08 14:59_

@zanieb @geofft 

Indeed! After updating uv everything works fine! Thank you very much!

---
