---
number: 10392
title: "`uv init --app` should create `main.py` instead of `hello.py`"
type: issue
state: closed
author: kdheepak
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-01-08T13:22:23Z
updated_at: 2025-01-08T14:32:19Z
url: https://github.com/astral-sh/uv/issues/10392
synced_at: 2026-01-10T01:24:53Z
---

# `uv init --app` should create `main.py` instead of `hello.py`

---

_Issue opened by @kdheepak on 2025-01-08 13:22_

I've been using `uv init --app` a lot more recently because I've deploying a script using [panel](https://panel.holoviz.org/). I consistently rename `hello.py` to `main.py`. 

I do think `hello.py` might a good beginner friendly name. I also think `main.py` would be equally obvious as to the fact that this is a script that is runnable. I think the following names would be all equally intuitive as an entrypoint.

- `main.py`
- `{project_name}.py`
- `cli.py`
- `script.py`
- `entrypoint.py`

The reasoning for this change is that I think `uv` should prefer that commonly used commands were optimized for long term or frequent users, while still being beginner friendly.

---

_Comment by @charliermarsh on 2025-01-08 14:32_

Thanks! I think this is a duplicate of #7782 which is addressed by #10369 (in the next minor release).

---

_Closed by @charliermarsh on 2025-01-08 14:32_

---

_Label `duplicate` added by @charliermarsh on 2025-01-08 14:32_

---

_Label `enhancement` added by @charliermarsh on 2025-01-08 14:32_

---
