---
number: 10677
title: "[Q&A] CI, pytest and extra vs env."
type: issue
state: open
author: ego-thales
labels: []
assignees: []
created_at: 2025-01-16T13:38:19Z
updated_at: 2025-01-16T13:39:08Z
url: https://github.com/astral-sh/uv/issues/10677
synced_at: 2026-01-07T13:12:18-06:00
---

# [Q&A] CI, pytest and extra vs env.

---

_Issue opened by @ego-thales on 2025-01-16 13:38_

Hello,

I have trouble understanding the content of the virtual environment used when running `uv run pytest`. Namely, during my CI pipeline, I test
- build
- install
- pytest

And I want to test installed package. What would be the way to do so?

For example, is the following what I want?
```bash
uv build
uv pip install dist/my-wheel.whl[extra]
uv run pytest
```

I'm not at all sure what to do. What's confusing to me is that the `uv` doc clearcly states that `dev` dependencies are installed by default in the virtual environment, but there's no similar mention for extras (whether they're installed).

Thanks in advance.

---

_Renamed from "CI, pytest and extra vs env." to "[Q&A] CI, pytest and extra vs env." by @ego-thales on 2025-01-16 13:39_

---
