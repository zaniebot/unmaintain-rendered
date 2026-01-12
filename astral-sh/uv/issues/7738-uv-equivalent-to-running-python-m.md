```yaml
number: 7738
title: "uv equivalent to running \"python -m\""
type: issue
state: closed
author: tomaszbk
labels:
  - question
assignees: []
created_at: 2024-09-27T13:09:13Z
updated_at: 2024-09-27T14:34:44Z
url: https://github.com/astral-sh/uv/issues/7738
synced_at: 2026-01-12T15:59:16Z
```

# uv equivalent to running "python -m"

---

_@tomaszbk_


I want to migrate to uv, and my actual project is under an "app/" folder, and we use python -m app.main.py to run it.

How can I achieve the same command using uv?

---

_Comment by @zanieb on 2024-09-27 13:19_

Generally, you'd use `uv run python -m app/main.py` — does that work for you?

---

_Comment by @zanieb on 2024-09-27 13:20_

Related https://github.com/astral-sh/uv/issues/6638

---

_Label `question` added by @zanieb on 2024-09-27 13:20_

---

_Comment by @tilschuenemann on 2024-09-27 13:25_

Adding onto this - how would I generate coverage when running pytest?
When using poetry I'd do `poetry run coverage run -m pytest tests/` .



---

_Comment by @zanieb on 2024-09-27 13:27_

It'd be the same, but with `uv run` instead of `poetry run`.

---

_Comment by @tomaszbk on 2024-09-27 14:10_

Yes it works! sorry for not seeing the related github issue before

---

_Closed by @zanieb on 2024-09-27 14:34_

---
