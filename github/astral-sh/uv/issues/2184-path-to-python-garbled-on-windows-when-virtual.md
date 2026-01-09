---
number: 2184
title: Path to python garbled on Windows when virtual environment activated
type: issue
state: closed
author: daniel-bartley
labels:
  - bug
  - windows
  - needs-mre
assignees: []
created_at: 2024-03-04T22:27:13Z
updated_at: 2024-03-13T10:15:11Z
url: https://github.com/astral-sh/uv/issues/2184
synced_at: 2026-01-07T13:12:17-06:00
---

# Path to python garbled on Windows when virtual environment activated

---

_Issue opened by @daniel-bartley on 2024-03-04 22:27_

Path to python gets messed up somewhere. I installed python310 with chocolatey.

```
$ python --version
Python 3.10.11

$ where python
C:\Python310\python.exe

$ pipx uninstall uv
uninstalled uv! ✨ 🌟 ✨

$ uv --version
bash: uv: command not found

$ pipx install uv
  installed package uv 0.1.14, installed using Python 3.10.11
  These apps are now globally available
    - uv.exe
done! ✨ 🌟 ✨

$ uv --version
uv 0.1.14 (c525fdf2b 2024-03-04)

$ uv venv
Using Python 3.10.11 interpreter at: C:\Users\myprofile\.local\bin\python3.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate


$ source .venv/Scripts/activate

(uv-validate) 
$ python --version
No Python at '"C:\Users\myprofile\.local\bin\python.exe'
```

Tried creating a shortcut to "python3.exe" named "python.exe" but that doesn't work


---

_Comment by @charliermarsh on 2024-03-04 22:30_

Thanks, I'll try to add some tests for this and see if we can reproduce.

---

_Label `bug` added by @charliermarsh on 2024-03-04 22:30_

---

_Label `windows` added by @charliermarsh on 2024-03-04 22:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-04 23:18_

---

_Comment by @charliermarsh on 2024-03-05 00:36_

I'm unable to reproduce locally, but what is `C:\Users\myprofile\.local\bin\python3.exe`?

---

_Comment by @dbrtly on 2024-03-05 04:03_

I'm honestly not sure. I think maybe pipx creates them for each app.  

---

_Comment by @dbrtly on 2024-03-05 10:39_

Installed cowsay via pipx. They definitely are artifacts from pipx. 

---

_Comment by @dbrtly on 2024-03-05 10:40_

I'm on pipx 1.4.3

---

_Unassigned @charliermarsh by @charliermarsh on 2024-03-08 01:14_

---

_Comment by @konstin on 2024-03-08 13:01_

Do you know where `.local\bin\python.exe` comes from? I tried various things but couldn't get one. I also tried using python 3.10 from `choco install python3.10`

```
PS C:\Users\Konstantin> dir .\.local\bin\

    Directory: C:\Users\Konstantin\.local\bin

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          08.03.2024    12:21         108418 cmake.exe
-a---          08.03.2024    12:21         108418 cpack.exe
-a---          08.03.2024    12:21         108418 ctest.exe
-a---          08.03.2024    12:21         108438 poetry.exe
la---          08.03.2024    12:43              0 pycowsay.exe -> C:\Users\Konstantin\AppData\Local\pipx\pipx\venvs\pycowsay\Scripts\pycowsay.exe
-a---          08.03.2024    12:52         108406 tqdm.exe
```

---

_Label `needs-mre` added by @konstin on 2024-03-08 13:01_

---

_Comment by @dbrtly on 2024-03-08 13:03_

I installed uv and many other python cli tools with pipx. Pipx is generating most of those files in that directory. 

---

_Comment by @dbrtly on 2024-03-08 13:05_

I tried installing pytthon with scoop at one point. Lemme try uninstalling python and going again over the weekend. 

---

_Comment by @daniel-bartley on 2024-03-09 06:17_

I deleted the python3.exe file and that seems to have solved the problem. How did that happen? lol

---

_Closed by @daniel-bartley on 2024-03-09 06:17_

---

_Reopened by @daniel-bartley on 2024-03-09 06:17_

---

_Comment by @charliermarsh on 2024-03-12 02:20_

Should this remain open @daniel-bartley?

---

_Comment by @dbrtly on 2024-03-13 08:08_

No happy to close. I think it was unique to me


Thanks,
Daniel
________________________________
From: Charlie Marsh ***@***.***>
Sent: Tuesday, March 12, 2024 1:21:14 PM
To: astral-sh/uv ***@***.***>
Cc: Daniel Bartley ***@***.***>; Comment ***@***.***>
Subject: Re: [astral-sh/uv] Path to python garbled on Windows when virtual environment activated (Issue #2184)


Should this remain open @daniel-bartley<https://github.com/daniel-bartley>?

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/2184#issuecomment-1989838532>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/ADLXPTNUTLDY3OOEMSRGSFDYXZYCVAVCNFSM6AAAAABEGBGFDOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMYTSOBZHAZTQNJTGI>.
You are receiving this because you commented.Message ID: ***@***.***>


---

_Closed by @konstin on 2024-03-13 10:15_

---
