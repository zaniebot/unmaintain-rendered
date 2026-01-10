---
number: 4991
title: "Question: can I use `uv venv` with vanilla pip"
type: issue
state: closed
author: mil-ad
labels:
  - question
assignees: []
created_at: 2024-07-11T11:31:25Z
updated_at: 2024-07-11T14:34:16Z
url: https://github.com/astral-sh/uv/issues/4991
synced_at: 2026-01-10T01:23:44Z
---

# Question: can I use `uv venv` with vanilla pip

---

_Issue opened by @mil-ad on 2024-07-11 11:31_

qq - can I create an and activate a venv with `uv venv` but use vanilla pip (from pyenv) instead of `uv pip`? When I try this packages are installed directly in pyenv's `lib/python3.x/site-packages`. Is this expected?

---

_Comment by @konstin on 2024-07-11 12:01_

You can use `uv venv --seed` to install `pip` into the virtual environment. Is there a reason you want to use `pip` instead of `uv pip`?

---

_Label `question` added by @konstin on 2024-07-11 12:01_

---

_Comment by @mil-ad on 2024-07-11 12:22_

I was wondering if I could just use the venv functinality of `uv` for projects that can't use `uv pip`, e.g. they use poetry or conda or something

---

_Closed by @zanieb on 2024-07-11 14:34_

---
