---
number: 60
title: Handle multiple unused submodule imports
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-08-31T20:33:40Z
updated_at: 2024-04-27T16:55:32Z
url: https://github.com/astral-sh/ruff/issues/60
synced_at: 2026-01-07T13:12:14-06:00
---

# Handle multiple unused submodule imports

---

_Issue opened by @charliermarsh on 2022-08-31 20:33_

pylint does this properly, Flake8 does not.

For example, with this:

```py
import multiprocessing.pool
import multiprocessing.process
```

Flake8 will only mark the second import as unused.


---

_Label `bug` added by @charliermarsh on 2022-08-31 20:33_

---

_Comment by @squiddy on 2022-11-09 10:42_

I have a question concerning a slightly adjusted example.

```python
import multiprocessing.pool
import multiprocessing.process

z = multiprocessing.pool.ThreadPool()
```

pylint and flake8 don't see any unused import here. I would argue `import multiprocessing.process` is.

*Update* Working on this, PR incoming.

---

_Referenced in [astral-sh/ruff#666](../../astral-sh/ruff/pulls/666.md) on 2022-11-10 06:03_

---

_Referenced in [astral-sh/ruff#3821](../../astral-sh/ruff/issues/3821.md) on 2023-03-30 18:02_

---

_Referenced in [astral-sh/ruff#4655](../../astral-sh/ruff/issues/4655.md) on 2023-06-10 16:15_

---

_Referenced in [astral-sh/ruff#5010](../../astral-sh/ruff/pulls/5010.md) on 2023-06-10 18:59_

---

_Referenced in [astral-sh/ruff#5011](../../astral-sh/ruff/pulls/5011.md) on 2023-06-11 02:28_

---

_Referenced in [astral-sh/ruff#7972](../../astral-sh/ruff/issues/7972.md) on 2023-10-16 03:30_

---

_Referenced in [astral-sh/ruff#8666](../../astral-sh/ruff/issues/8666.md) on 2023-11-14 00:12_

---

_Comment by @ThiefMaster on 2023-11-14 00:17_

Would you consider exempting F401 from RUF100 until this is solved?

Even if ruff cannot handle this correctly, it'd be much cleaner if as a developer one could still add the proper noqa on each such unused import simply to indicate to other developers that they are aware of it being unused.

---

_Referenced in [astral-sh/ruff#8492](../../astral-sh/ruff/issues/8492.md) on 2023-11-14 13:28_

---

_Referenced in [astral-sh/ruff#10674](../../astral-sh/ruff/issues/10674.md) on 2024-03-30 23:12_

---

_Comment by @JonathanPlasse on 2024-04-27 16:55_

Will Red knot address this issue?

---

_Referenced in [astral-sh/ruff#13000](../../astral-sh/ruff/issues/13000.md) on 2024-08-20 03:11_

---

_Referenced in [astral-sh/ruff#13965](../../astral-sh/ruff/pulls/13965.md) on 2024-10-28 14:55_

---

_Referenced in [astral-sh/ruff#14127](../../astral-sh/ruff/issues/14127.md) on 2024-11-06 11:39_

---

_Referenced in [astral-sh/ruff#18893](../../astral-sh/ruff/issues/18893.md) on 2025-06-23 11:58_

---

_Referenced in [astral-sh/ruff#18985](../../astral-sh/ruff/issues/18985.md) on 2025-06-27 16:00_

---

_Referenced in [astral-sh/ruff#20200](../../astral-sh/ruff/pulls/20200.md) on 2025-09-12 19:04_

---
