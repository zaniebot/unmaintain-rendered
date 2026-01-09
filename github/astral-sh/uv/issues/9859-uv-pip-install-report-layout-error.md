---
number: 9859
title: uv pip install report layout error
type: issue
state: closed
author: cjdxhjj
labels:
  - question
assignees: []
created_at: 2024-12-13T03:28:09Z
updated_at: 2024-12-13T04:00:42Z
url: https://github.com/astral-sh/uv/issues/9859
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip install report layout error

---

_Issue opened by @cjdxhjj on 2024-12-13 03:28_

i have use uv init, uv add -r ./requement.txt to transform my project from requirment to uv use pyproject.toml to manager deps
![Image](https://github.com/user-attachments/assets/91c000cf-310c-4990-948e-41031d7e9bfa)
my project has that layout
when i install the deps. use uv venv and uv pip install . it report the error
![Image](https://github.com/user-attachments/assets/22aaad55-b6a2-488f-8d70-b742d0cbbd0a)
the project is web project and not to plan publish as a package

---

_Comment by @charliermarsh on 2024-12-13 03:29_

You should use `uv sync` here, rather than `uv pip install .`. If you want to use `uv pip install`, it should be `uv pip instal -r pyproject.toml` since you don't want the project itself to be installed. But if you use `uv pip install`, you won't get the benefits of `uv.lock`.

---

_Label `question` added by @charliermarsh on 2024-12-13 03:29_

---

_Comment by @cjdxhjj on 2024-12-13 03:29_

i must delete the pyproject.toml, use this step to install deps
uv venv
uv add -r ./requirment.txt

---

_Comment by @cjdxhjj on 2024-12-13 03:30_

i will have a try

---

_Comment by @cjdxhjj on 2024-12-13 04:00_

it works, thanks @charliermarsh 

---

_Closed by @cjdxhjj on 2024-12-13 04:00_

---
