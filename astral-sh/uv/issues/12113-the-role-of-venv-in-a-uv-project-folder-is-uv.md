---
number: 12113
title: "The role of venv in a uv project folder: (Is  \"uv venv --python <version>\" or \"source .venv/bin/activated\" needed?)"
type: issue
state: closed
author: zhyeong
labels:
  - question
assignees: []
created_at: 2025-03-11T12:02:38Z
updated_at: 2025-03-14T00:33:17Z
url: https://github.com/astral-sh/uv/issues/12113
synced_at: 2026-01-10T01:25:15Z
---

# The role of venv in a uv project folder: (Is  "uv venv --python <version>" or "source .venv/bin/activated" needed?)

---

_Issue opened by @zhyeong on 2025-03-11 12:02_

### Question

Hi, I was wondering when `uv venv --python <version>` command is useful when `.python-version` file exists, since the python version I set up in venv always changes following the `.python-version` file. 

I was testing with the settings below to see how python version control works.

1. pyproject.toml (manually edited as below)
`requires-python = "<=3.13.2"`

2..python-version (manually edited as below)
`3.9.6`

Now, if I run `uv venv --python 3.13.2`, I can see that python 3.13 is installed in .venv/lib. also, I can check that 3.13.2 is installed in venv. 


```
(uv-test) user@XX % uv venv --python 3.9.6
>> [result not pasted here]
(uv-test) user@XX % python --version
>> Python 3.13.2 
```

But if run `uv run main.py`, where `main.py` is like below:

```
import sys
print(sys.version)
```

The printed result is `3.9.6`, and I see Python 3.9 installed in .venv/lib instead of 3.13. (python version in venv changed according to .python-version)

If python version written in `.python-version` is prioritized over what I installed using `uv venv --python <version>`, when should `uv venv --python <version>` used?

Thank you so much for reading my question, and I appreciate any help! 



### Platform

Darwin 24.2.0 arm64

### Version

uv 0.6.5

---

_Label `question` added by @zhyeong on 2025-03-11 12:02_

---

_Comment by @zanieb on 2025-03-11 16:33_

When you're in a project, we'll automatically manage the virtual environment — replacing it as needed. In this context, a `.python-version` file takes precedence over the existing virtual environment. You can consider the example:

- You are working on a project, `uv run ...` creates a virtual environment
- Someone changes the `.python-version`
- You pull the change
- `uv run` should create a new virtual environment, so it continues to "just work"

Creating virtual environments manually with `uv venv ` is intended for use _outside_ of projects, or in projects that are not managed by uv. These virtual environments are manually managed via `uv pip`.



---

_Comment by @zhyeong on 2025-03-12 00:51_

Thank you so much for your quick and thorough response!

So I understand that you can 
A)  `uv init` in a project folder and `uv run` to install venv which will follow the python version of the uv project, or
B) `uv venv` in a folder to create venv but not uv project (so no `.python-version`)

My question is,

In an uv project folder, is there distinction between when you should run commands with venv activated and deactivated?

I noticed that even with venv deactivated your codes can still find the packages installed in venv. So if in a uv project folder, do. I have to activate venv? 
 


---

_Renamed from "When to use "uv venv --python <version>" when there is ".python-version" file" to "The role of venv in a uv project folder: (Is  "uv venv --python <version>" or "source .venv/bin/activated" needed?)" by @zhyeong on 2025-03-12 00:52_

---

_Comment by @zanieb on 2025-03-12 01:18_

You don't need to activate it unless you want to bypass `uv run` and invoke commands directly.

---

_Closed by @charliermarsh on 2025-03-14 00:33_

---

_Assigned to @zanieb by @charliermarsh on 2025-03-14 00:33_

---
