```yaml
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
synced_at: 2026-01-10T11:09:42Z
```

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

_Comment by @ThiefMaster on 2023-11-14 00:17_

Would you consider exempting F401 from RUF100 until this is solved?

Even if ruff cannot handle this correctly, it'd be much cleaner if as a developer one could still add the proper noqa on each such unused import simply to indicate to other developers that they are aware of it being unused.

---

_Comment by @JonathanPlasse on 2024-04-27 16:55_

Will Red knot address this issue?

---
