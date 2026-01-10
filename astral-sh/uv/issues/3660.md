```yaml
number: 3660
title: "'Unable to create Windows launch for x86' while installing packages"
type: issue
state: closed
author: anishpillay2002
labels:
  - windows
assignees: []
created_at: 2024-05-19T23:50:20Z
updated_at: 2024-05-28T16:07:40Z
url: https://github.com/astral-sh/uv/issues/3660
synced_at: 2026-01-10T05:31:37Z
```

# 'Unable to create Windows launch for x86' while installing packages

---

_Issue opened by @anishpillay2002 on 2024-05-19 23:50_

I receive the following error while trying to install packages with uv but `pip install crewai` works fine

I've also attached a text file with the output with using verbose flag.
[verbose_output.txt](https://github.com/astral-sh/uv/files/15371152/verbose_output.txt)
```
PS D:\Temp\test> py -3.11 -m uv venv
Using Python 3.8.2 interpreter at: C:\Python38\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
PS D:\Temp\test> py -3.11 -m uv venv --python 3.11
Using Python 3.11.0 interpreter at: C:\Users\Anish Pillay\AppData\Local\Programs\Python\Python311\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
PS D:\Temp\test> .\.venv\Scripts\activate
(test) PS D:\Temp\test> uv pip install crewai
error: Failed to download and build `pypika==0.48.9`
  Caused by: Failed to build: `pypika==0.48.9`
  Caused by: Failed to install requirements from build-system.requires (install)
  Caused by: Failed to install build dependencies
  Caused by: Failed to install: wheel-0.43.0-py3-none-any.http.whl (wheel==0.43.0)
  Caused by: Unable to create Windows launch for x86 (only x64_64 is supported)
(test) PS D:\Temp\test> uv --version
uv 0.1.44 (d417daad7 2024-05-14)
(test) PS D:\Temp\test> python --version
Python 3.11.0
```



---

_Label `windows` added by @charliermarsh on 2024-05-20 00:35_

---

_Comment by @charliermarsh on 2024-05-20 00:35_

Unclear if we'll add support for 32-bit launchers.

---

_Comment by @zakimimit on 2024-05-27 13:37_

the same problem 

Resolved 28 packages in 19ms
error: Failed to install: auto_py_to_exe-2.43.3-py2.py3-none-any.http.whl (auto-py-to-exe==2.43.3)
  Caused by: Unable to create Windows launch for x86 (only x64_64 is supported)
  
  
  i hope you fix it
 
  
  
  

---

_Comment by @zakimimit on 2024-05-27 17:05_

My only solution is remove uv from the x86 version of python 
so the when call uv pip ... it works perfectly without error 

only problem that you will face is "pip install --upgrade pip"
is not working and you can't upgrade it I don't know if there is any problem that can appear if there is no update because the uv handles the installation of packages.

I hope the team explain more the tool and how the tool handles the installation of packages.

My best regards 

and thank for the dev team for this valuable tool
@anishpillay2002 
@charliermarsh 

---

_Comment by @charliermarsh on 2024-05-27 17:08_

Thanks @zakimimit. We'll look into supporting x86 launchers, at least see how hard it is.

Can you repeat your question? I'm having trouble following it exactly.

---

_Comment by @zakimimit on 2024-05-27 17:14_

the is problem that "pip install --upgrade pip"
is not working and you can't upgrade it 
I don't know if there is any problem that can appear if there is no update because the uv handles the installation of packages.

so my question that when  I create virtual env , I always use pip install --upgrade pip.

is there any problem I will face if the don't use it because uv not support it , and the uv handle the insatlaion unstand of pip .

if you can give a bref explaniation of how uv tool handles the installation of packages.

so we get clear vison of how we use it 
@charliermarsh


---

_Comment by @charliermarsh on 2024-05-27 17:24_

Are you using `uv venv` to create a virtualenv, then `pip install --upgrade pip` inside it? If so, try `uv venv --seed` instead of `uv venv`.

---

_Comment by @zakimimit on 2024-05-27 17:31_

yes i use uv venv

1- so they are no need of pip  installer ? 
2- can you explain what the uv venv --seed dose ?

@charliermarsh 

---

_Closed by @charliermarsh on 2024-05-28 16:07_

---
