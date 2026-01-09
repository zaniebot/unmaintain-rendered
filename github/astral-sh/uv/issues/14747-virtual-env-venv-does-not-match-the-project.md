---
number: 14747
title: "VIRTUAL_ENV=.venv` does not match the project environment path.  Forced to use --active"
type: issue
state: closed
author: Allena101
labels:
  - question
assignees: []
created_at: 2025-07-19T14:07:50Z
updated_at: 2025-12-03T10:03:50Z
url: https://github.com/astral-sh/uv/issues/14747
synced_at: 2026-01-07T13:12:19-06:00
---

# VIRTUAL_ENV=.venv` does not match the project environment path.  Forced to use --active

---

_Issue opened by @Allena101 on 2025-07-19 14:07_

### Question

I keep getting this messege now and i cannot figure out why. There is a chance it has to do with the mentioned vscode workspace , but I cant figure out how to close that either.

I tried creating several new projects in new folders and also giving the environment a different name (e.g. myVENV) and i get the exact same message each time.

Googling the issue as well as asking llm Models does not make me understand what is going on nor give a solution for a fix.

Hope the terminal message explains enough:


Adding `adk-interview-assistant-7` as member of workspace `/home/magnus`
Initialized project `adk-interview-assistant-7`
(.venv) magnus@DESKTOP-4ME2A7J:~/code_projects/adk_interview_assistant_7$ uv add google-adk ipykernel
warning: `VIRTUAL_ENV=.venv` does not match the project environment path `/home/magnus/.venv` and will be ignored; use `--active` to target the active environment instead

### Platform

wsl ubuntu

### Version

_No response_

---

_Label `question` added by @Allena101 on 2025-07-19 14:07_

---

_Comment by @Allena101 on 2025-07-19 14:23_

After some hours fiddling i am pretty sure that workspace is the issue. Though now i cannot figure out how to delete this workspace. It seems like my root project folder (i mean the folder that i have all my projects in) are no all part of a workspace. And the option of right clicking on a folder and removing it from workspace does not show up from me. I also dont see any of the workspace files anywhere. I only get mentions of workspace once i try using uv add.

So this might just be me not being able to figure out vscode workspace. 

Pip install seems to work without this issue though.

---

_Comment by @Allena101 on 2025-07-19 14:29_

OK, i did find the remove folder from workspace command but all this does it throw me out of the project folder and back to my root wsl folder. So i have no idea what is going on

---

_Closed by @Allena101 on 2025-07-19 15:02_

---

_Comment by @HasTarC0d3 on 2025-07-25 13:15_

Continously facing this problem with all the project I am working after starting using uv recently. I am getting the message:

> uv add pkgname --group dev
warning: `VIRTUAL_ENV=.venv` does not match the project environment path `/Users/username/.venv` and will be ignored; use `--active` to target the active environment instead

While trying to ignore this, when I try to `uv add` and `uv sync` it is deleting my current .venv folder and recreating with the version of `/Users/username/.venv`. 

```
uv add pkgname --active --group dev
Using CPython 3.13.1
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```
Totally not helping and missing the whole point of creating virtual environment. If anyone can solve this, please help.

Platform: MacOS 15.5

---

_Comment by @zanieb on 2025-07-25 13:21_

You should just `deactivate` the environment? Can you say more about what you're trying to do?

---

_Comment by @Allena101 on 2025-07-26 13:19_

> You should just `deactivate` the environment? Can you say more about what you're trying to do?

Hi  👋 Sorry I did solve this issue before. I did click the "close issue" button but maybe i am misunderstanding the proper protocol to use here. 

Anyways, it turned out that I had ran uv init in my WSL root folder. What confused me alot , and i tried to explain here, is when the error message mentions workspace which made me think it had to do with vscode and not uv. 

So when i deleted the uv files in my wsl root folder everything went back to normal. 

---

_Comment by @vykhovanets on 2025-12-03 09:58_

Hi!

I have global `.venv` in the root, that is activated, not to polute system python installations, and also I have the `.venv` in each uv-based project, and when I do `uv run ..` I see such warning - It doesn't matter, but would be cool to be able to suppress it somehow.

because for me having `.venv` in a root activated by default is not an error, but a way I work.

What I get:
```sh
warning: `VIRTUAL_ENV=/Users/<user>/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
```


---
