---
number: 7848
title: Provide tk installation with libXft support in Python installations
type: issue
state: open
author: FCamborda
labels: []
assignees: []
created_at: 2024-10-01T16:25:16Z
updated_at: 2025-08-08T18:19:35Z
url: https://github.com/astral-sh/uv/issues/7848
synced_at: 2026-01-10T01:24:20Z
---

# Provide tk installation with libXft support in Python installations

---

_Issue opened by @FCamborda on 2024-10-01 16:25_

We're trying to use customtkinter (which depends on tkinter, already delivered with the uv Python installation). As seen in this issue, we export the necessary env. variables to get tkinter to work (https://github.com/astral-sh/uv/issues/7036) 

```shell
uv python install 3.9
uv venv testenv --python 3.9
BASE_PATH="$(dirname $(dirname $(readlink $(which python))))"
export TK_LIBRARY="$BASE_PATH/lib/tk8.6"
export TCL_LIBRARY="$BASE_PATH/lib/tcl8.6"
uv pip install customtkinter
python
```

Then call the following code (taken from https://customtkinter.tomschimansky.com)
```python
import customtkinter

def button_callback():
    print("button clicked")

app = customtkinter.CTk()
app.geometry("400x150")

button = customtkinter.CTkButton(app, text="my button", command=button_callback)
button.pack(padx=20, pady=20)

app.mainloop()
```
The rendering is then quite ugly.  Apparently this is a known issue when tk is built without libXft support:
https://github.com/TomSchimansky/CustomTkinter/issues/2596

Could you confirm that this is the case when installing Python in uv? If so, is there a way to include this to make rendering more pleasent to the eyes?

We are using uv 0.4.9 in native Ubuntu 22.04.4 LTS

Thanks.

---

_Comment by @learncodingforweb on 2024-12-28 15:38_

I am also facing same problem running on ubuntu 24.04LTS with different version (other than system python version) install using uv python install

```
uname -a
Linux embedded 6.8.0-51-generic #52-Ubuntu SMP PREEMPT_DYNAMIC Thu Dec  5 13:09:44 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux``

system has python version 
```
python --version
Python 3.12.3
which python
/usr/bin/python```

for which tkinter is working fine.

But i install python other than system version. than uv is not able to find tkinter and getting error
**This probably means that Tcl wasn't installed properly.**

---

_Comment by @zanieb on 2024-12-28 17:48_

@learncodingforweb this sounds like a separate issue. Please see #9452 / open a new issue including the output.. Make sure you're on the latest version of uv and reinstall your managed Python version with `uv python install <version> --reinstall` to ensure you have the latest patches.

---

_Comment by @learncodingforweb on 2024-12-29 12:54_

i already have latest version. 
```
uv --version
uv 0.5.13
```
I even tried
```
uv python uninstall 3.13
uv python install 3.13
```
Still not working

---

_Comment by @zanieb on 2024-12-29 16:16_

@learncodingforweb again, please open a new issue including a reproduction and output as described in #9452. It's bad form to hijack another issue.

---

_Referenced in [astral-sh/python-build-standalone#740](../../astral-sh/python-build-standalone/issues/740.md) on 2025-08-08 18:18_

---

_Comment by @geofft on 2025-08-08 18:19_

> Could you confirm that this is the case when installing Python in uv?

Yes, it is the case that our managed Python installs are built without Xft. I guess we ought to do that. I opened astral-sh/python-build-standalone#740, not sure when I'll get to it.

---
