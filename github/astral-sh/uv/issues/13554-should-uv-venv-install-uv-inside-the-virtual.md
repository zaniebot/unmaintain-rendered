---
number: 13554
title: "Should `uv venv` install uv inside the virtual environment it creates (when uv is not installed globally)?"
type: issue
state: closed
author: chirizxc
labels:
  - question
assignees: []
created_at: 2025-05-20T13:04:28Z
updated_at: 2025-05-20T13:08:49Z
url: https://github.com/astral-sh/uv/issues/13554
synced_at: 2026-01-07T13:12:18-06:00
---

# Should `uv venv` install uv inside the virtual environment it creates (when uv is not installed globally)?

---

_Issue opened by @chirizxc on 2025-05-20 13:04_

### Question

![Image](https://github.com/user-attachments/assets/72677dfa-7091-4d38-b0af-c1755c551dec)

![Image](https://github.com/user-attachments/assets/e941fb3a-6af2-44e7-8c75-1f734f200d4e)
```bash
python -m venv py_venv
py_venv\Scripts\activate
pip install uv
uv venv uv_venv
uv_venv\Scripts\activate
uv --version# =>  `uv` is not recognized
```

### Platform

Windows 10

### Version

uv 0.7.6 (7f3e94a09 2025-05-19)

---

_Label `question` added by @chirizxc on 2025-05-20 13:04_

---

_Comment by @charliermarsh on 2025-05-20 13:08_

We intentionally do _not_ do this, since there's no need for uv to be installed inside of a virtual environment for it to manipulate it. Adding it to the environment would lead to confusion.

```
uv venv uv_venv
uv_venv\Scripts\activate
uv --version# =>  `uv` is not recognized
```

Typically, you'd install uv globally, and not in an environment in the first place.

---

_Closed by @chirizxc on 2025-05-20 13:08_

---
