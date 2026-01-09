---
number: 1741
title: pip list looks at global and not .venv created with UV
type: issue
state: closed
author: ion-elgreco
labels: []
assignees: []
created_at: 2024-02-20T09:08:10Z
updated_at: 2024-02-20T19:00:05Z
url: https://github.com/astral-sh/uv/issues/1741
synced_at: 2026-01-07T13:12:16-06:00
---

# pip list looks at global and not .venv created with UV

---

_Issue opened by @ion-elgreco on 2024-02-20 09:08_

Running  python3 -m venv .venv, and then install with uv pip install works fine. You can then also inspect the installed packages with pip list

However doing `uv venv` then install with `uv pip install` also works, but now I cannot do pip list even though .venv is activated. Pip list is now looking at global install.

---

_Comment by @m-aciek on 2024-02-20 10:32_

What does `python -m pip list` return for you?

---

_Comment by @ion-elgreco on 2024-02-20 10:39_

@m-aciek it returns this: 

```python
(.venv) ion@<redacted>:~/<redacted>$ python -m pip list
/home/ion/<redacted>/.venv/bin/python: No module named pip
```

---

_Comment by @m-aciek on 2024-02-20 10:50_

You don't have pip installed in your venv. You need to install it there or wait for https://github.com/astral-sh/uv/issues/1401.

---

_Closed by @ion-elgreco on 2024-02-20 10:52_

---

_Comment by @hauntsaninja on 2024-02-20 19:00_

You can use uv pip freeze instead for now. Also note that if you pass --seed to uv venv it will put pip in the environment 

---
