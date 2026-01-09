---
number: 8354
title: uv venv 创建的虚拟环境，  bin目录下不包含pip，可能会存在虚拟环境的 python版本和pip 不对应的问题
type: issue
state: closed
author: shell-nlp
labels:
  - question
assignees: []
created_at: 2024-10-19T02:08:28Z
updated_at: 2024-10-19T02:28:40Z
url: https://github.com/astral-sh/uv/issues/8354
synced_at: 2026-01-07T13:12:17-06:00
---

# uv venv 创建的虚拟环境，  bin目录下不包含pip，可能会存在虚拟环境的 python版本和pip 不对应的问题

---

_Issue opened by @shell-nlp on 2024-10-19 02:08_

uv 0.4.22

uv venv 创建的虚拟环境，  bin目录下不包含pip，可能会存在虚拟环境的 python版本和pip 不对应的问题，导致 pip list 命令展示出系统的 包列表，而不是虚拟环境本身的。

---

_Comment by @zanieb on 2024-10-19 02:18_

Sorry, I can't read this. I tried ChatGPT

> This text discusses a potential issue with virtual environments created using uv venv. It states that the bin directory of the virtual environment may not include pip, which could lead to a mismatch between the Python version used in the virtual environment and the version of pip. As a result, running the pip list command might show the system-wide packages instead of those installed in the virtual environment itself.

If that's correct, you want `uv venv --seed` — similar to https://github.com/astral-sh/uv/issues/7534

---

_Label `question` added by @zanieb on 2024-10-19 02:19_

---

_Comment by @shell-nlp on 2024-10-19 02:28_

Nice, it solved my problem.

---

_Closed by @zanieb on 2024-10-19 02:28_

---
