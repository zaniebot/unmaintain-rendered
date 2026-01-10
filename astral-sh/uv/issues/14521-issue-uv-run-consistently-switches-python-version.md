---
number: 14521
title: "Issue: ```uv run``` consistently switches Python version to system default, recreating venv"
type: issue
state: closed
author: Muhammad-Saad-2
labels:
  - question
assignees: []
created_at: 2025-07-09T14:49:19Z
updated_at: 2025-07-09T15:26:49Z
url: https://github.com/astral-sh/uv/issues/14521
synced_at: 2026-01-10T01:25:45Z
---

# Issue: ```uv run``` consistently switches Python version to system default, recreating venv

---

_Issue opened by @Muhammad-Saad-2 on 2025-07-09 14:49_



### Problem Description

When attempting to execute a Python script using ```uv run``` within an already activated virtual environment, ```uv run``` exhibits highly unexpected and disruptive behavior which eventually leads my code to crash  since one of the packages known ```browser-use``` only supports ```pyhton 3.11``` and above, Anyways, Instead of utilizing the Python interpreter from the active virtual environment, it consistently reverts to the system's default Python interpreter on my machine```Python 3.10.12``` on``` Ubuntu 22.04 -jammy-jellyfish``` and so does on my friend's machine who has ```Ubuntu 24.04 - Noble Numbat``` which eventually worked since ```Ubuntu 24.04``` comes with ```python 3.12``` interpreter Furthermore, during this process, ```uv run``` appears to actively remove and then immediately recreate the virtual environment. This problematic behavior occurs even when the virtual environment was explicitly created with a specific, different Python version ```Python 3.13.5 ```in my case and has been confirmed to be properly activated and correctly configured in the shell.

Though running it directly with ```python main.py``` does works but it negates some of the convenience and potential performance benefits ```uv run``` is designed to offer, but once you try to run it with ```uv run``` the python version that you created the ```venv``` with  would be overridden with the machine's default python and you would have to create a virtual environment with an updated python version again and again

The selection below from my terminal would be enough to explain the issue in the most simplest way as possible, you can clearly read  ```Removed virtual environment at: .venv``` in the terminal few lines down, let me know if this is just the common behavior.



```
muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ uv venv --python 3.13 .venv
Using CPython 3.13.5 interpreter at: /usr/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ source .venv/bin/activate
(.venv) muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ python --version
Python 3.13.5
(.venv) muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ uv pip install browser-use --quiet
(.venv) muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ uv run main.py
Using CPython 3.10.12 interpreter at: /usr/bin/python3.10
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "/home/muhammad-saad/work/projects/doc_reader/main.py", line 1, in <module>
    from browser_use.llm import ChatGoogle
ModuleNotFoundError: No module named 'browser_use'
(.venv) muhammad-saad@muhammadsaad-ThinkPad-T440:~/work/projects/doc_reader$ 

```


### Platform

Ubuntu 22.04 arm64 - jammy-jellyfish

### Version

uv 0.6.11

### Python version

3.13.5

---

_Label `bug` added by @Muhammad-Saad-2 on 2025-07-09 14:49_

---

_Comment by @zanieb on 2025-07-09 15:07_

Does your project have a `.python-version` pin? (you could view it with `uv python pin`)

If so, that will be an implicit request to `uv run` and it will create a new environment with that version if necessary.

---

_Label `bug` removed by @zanieb on 2025-07-09 15:07_

---

_Label `question` added by @zanieb on 2025-07-09 15:07_

---

_Closed by @Muhammad-Saad-2 on 2025-07-09 15:26_

---
