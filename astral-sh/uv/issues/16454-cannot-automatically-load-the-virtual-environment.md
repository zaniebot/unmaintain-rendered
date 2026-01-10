---
number: 16454
title: "Cannot Automatically Load the Virtual Environment's Interpreter in VSCODE"
type: issue
state: closed
author: yiyuanli939
labels:
  - external
assignees: []
created_at: 2025-10-26T20:21:34Z
updated_at: 2025-10-30T13:06:26Z
url: https://github.com/astral-sh/uv/issues/16454
synced_at: 2026-01-10T01:26:06Z
---

# Cannot Automatically Load the Virtual Environment's Interpreter in VSCODE

---

_Issue opened by @yiyuanli939 on 2025-10-26 20:21_

### Summary

Now, uv cannot automatically load the interpreter in my vscode. My current workflow is something like 
```
uv venv 
source .venv/bin/activate 
uv sync 
```
At this moment, I can see the virtual environment in my vscode's terminal. However, when I was trying to select the interpreter using <command> `<command> + <shift> + P`, the interpreter is never automatically loaded. Even if I reloaded the window using "Python: Clear Cache and Reload Window", the problem persists. Why this happen? How can it  be solved? (asked Claude before raising this issue). 

### Platform

macOS 16 arm64

### Version

uv 0.9.2 (141369ce7 2025-10-10)

### Python version

Python 3.13.7

---

_Label `bug` added by @yiyuanli939 on 2025-10-26 20:21_

---

_Comment by @yiyuanli939 on 2025-10-26 20:32_

With further experiment, I think a issue is when I set the uv project as the root directory by open `my_uv_project`, the python interpreter can fetch my interpreter properly. But if I open a new folder, then use `git clone <forked_project>`, `cd forked_project`, then `uv venv`..., the search of the interpreter becomes impossible. 

---

_Comment by @lagamura on 2025-10-28 12:57_

Possibly this is not a uv issue, but a vs-code one. Especially in combination with the new python-env extension, as there have been several issues there.
You may check extension's repo here: https://github.com/microsoft/vscode-python-environments/issues

---

_Comment by @konstin on 2025-10-28 13:45_

Is this the same problem as https://github.com/astral-sh/uv/issues/9637? This does look like a VS Code and not a uv problem.

---

_Label `bug` removed by @konstin on 2025-10-28 13:45_

---

_Label `external` added by @konstin on 2025-10-28 13:45_

---

_Comment by @yiyuanli939 on 2025-10-30 02:57_

Thanks for the clarification! 

---

_Comment by @lagamura on 2025-10-30 10:48_

I think this one can be closed then, @yiyuanli939 feel free to do if no further points.

---

_Closed by @konstin on 2025-10-30 13:06_

---
