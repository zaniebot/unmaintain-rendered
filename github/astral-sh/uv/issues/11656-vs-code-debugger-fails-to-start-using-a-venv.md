---
number: 11656
title: VS Code debugger fails to start using a venv created by uv
type: issue
state: closed
author: StefanHemetsberger
labels:
  - bug
assignees: []
created_at: 2025-02-20T07:31:58Z
updated_at: 2025-02-21T08:06:12Z
url: https://github.com/astral-sh/uv/issues/11656
synced_at: 2026-01-07T13:12:18-06:00
---

# VS Code debugger fails to start using a venv created by uv

---

_Issue opened by @StefanHemetsberger on 2025-02-20 07:31_

### Summary

Yesterday I broke my VS Code setup in a project I am working on.

Do find the issue I created a very simple project with one main.py file which I wanted to debug in VS Code.
I also reinstalled VS Code and the Python Debug Extensions.
Additionally I installed multiple older Versions of the Python Debug Extension.

Commands I used to create the venv:

```
uv init
uv sync
```


Then I set the Interpretor in VS Code and configured the launch.json.

Starting now the Debugger, nothing happend.
The debugger immediately stops.

When switching to another virtual environment installed on my machine (conda), the debugger works as expected.


After some tests I figured out that maybe the "uv sync" was the problem.
When using this commands:

```
uv init
uv venv
```

...it seems to work.

Is this as expected or a bug?

Maybe it will help the one or other in the future.

### Platform

Microsoft Windows Server 2022 Standard
Version 10.0.20348 Build 20348

### Version

0.6.2

### Python version

3.12.8


### VS Code version

1.97.2

### Python Extension version

v2025.0.0


### Python Debugger Extension version

v2025.0.1



---

_Label `bug` added by @StefanHemetsberger on 2025-02-20 07:31_

---

_Comment by @StefanHemetsberger on 2025-02-20 08:13_

I know faced the same issue in another project where I am switched to UV now.
There I saw that the python interpretor points to ".\.venv\Scripts\python" after creating the venv with uv.
This does not work and the VS Code debugger wont start.

On my working environment it points to ".\.venv\Scripts\python.exe".

Can this be the issue?
After I changed it it worked.



---

_Comment by @StefanHemetsberger on 2025-02-21 08:06_

This issue can be closed.
The problem was the VS Code interpretor path.
Running uv commands in a different order seems to fix my problem, but I was wrong.

---

_Closed by @StefanHemetsberger on 2025-02-21 08:06_

---
