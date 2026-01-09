---
number: 2317
title: uv Created Virtual Environment is not Usable by PyCharm
type: issue
state: closed
author: bwanshoom
labels:
  - bug
  - windows
assignees: []
created_at: 2024-03-09T12:01:44Z
updated_at: 2024-03-09T14:43:20Z
url: https://github.com/astral-sh/uv/issues/2317
synced_at: 2026-01-07T13:12:17-06:00
---

# uv Created Virtual Environment is not Usable by PyCharm

---

_Issue opened by @bwanshoom on 2024-03-09 12:01_

PyCharm will not recognize a virtual environment created by uv. I'm not sure what it's looking for in a virtual environment, but one created by python -m venv .venv is somehow different from one created by uv venv --seed.

I can select the uv created environment, but PyCharm won't use it, save it or load it.

---

_Comment by @bwanshoom on 2024-03-09 12:05_

Answering my own issue - uv does not create an Include directory like the Python venv module does. That directory is empty with the python venv, so just creating an empty Include directory in the uv venv allows PyCharm to use the uv virtual environment.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-09 12:52_

---

_Comment by @charliermarsh on 2024-03-09 12:55_

The thing I'm surprised by is that `virtualenv` doesn't create an `include` either, but `python -m venv` does. Do you see the same problem if you use `virtualenv`?

---

_Comment by @charliermarsh on 2024-03-09 13:04_

Can you give me steps to repro? Specifically, what do you do in the PyCharm UI that isn't working?

---

_Label `needs-mre` added by @charliermarsh on 2024-03-09 13:04_

---

_Comment by @bwanshoom on 2024-03-09 13:09_

Yes, it's on Windows. Interestingly, virtualenv does not fail even without the Include directory.

---

_Comment by @bwanshoom on 2024-03-09 13:14_

Repro steps:
1. In PyCharm, open Settings => Python Interpreter
2. Click Add Interpreter => Add Local Interpreter
3. Click Existing then browse to the uv venv (e.g. xxxx\\.venv\Scripts\python.exe)
4. Click OK, then click OK again
5. If PyCharm likes the interpreter, it will read the installed packages and display them. With the uv one, it won't show anything. If you close settings and go back in, there will be no interpreter selected for the project


---

_Comment by @charliermarsh on 2024-03-09 13:46_

Thanks, I can reproduce on Windows! (But not macOS.)

---

_Label `needs-mre` removed by @charliermarsh on 2024-03-09 13:46_

---

_Label `bug` added by @charliermarsh on 2024-03-09 13:46_

---

_Label `windows` added by @charliermarsh on 2024-03-09 13:46_

---

_Comment by @charliermarsh on 2024-03-09 13:54_

Hmm, I actually can't reproduce this though when I use `--seed`.

---

_Comment by @charliermarsh on 2024-03-09 13:55_

Can you try removing the interpreter from PyCharm, re-running with `--seed`, re-opening PyCharm, and trying again...?

---

_Comment by @bwanshoom on 2024-03-09 14:02_

I did that originally and it wasn't working, but now after doing it with a new project it is working. I think I was running into some kind of internal caching in PyCharm so this is probably not a uv issue after all.

---

_Comment by @charliermarsh on 2024-03-09 14:43_

Yeah, I see similar flakiness in the caching. I had to remove the interpreter and restart. Anyway, thank you for filing.

---

_Closed by @charliermarsh on 2024-03-09 14:43_

---
