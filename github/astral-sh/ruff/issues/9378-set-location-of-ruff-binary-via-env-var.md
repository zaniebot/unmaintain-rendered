---
number: 9378
title: Set location of ruff binary via env var
type: issue
state: closed
author: jenia90
labels:
  - question
assignees: []
created_at: 2024-01-03T10:37:54Z
updated_at: 2024-01-29T13:59:36Z
url: https://github.com/astral-sh/ruff/issues/9378
synced_at: 2026-01-07T13:12:15-06:00
---

# Set location of ruff binary via env var

---

_Issue opened by @jenia90 on 2024-01-03 10:37_

Our python dist uses a different location for the bin/ folder, thus getting its location via `sysconfig` functions doesn't work.
So, it'd be really handy to be able to specify the location of ruff executable via env var. We can have something like
`RUFF_BIN_PATH=/some/path/ruff`

If you are ok with that, I can send a PR.

Thanks for all the awesome job you're doing!

---

_Comment by @charliermarsh on 2024-01-03 14:40_

Are you referring here to invocations of `python -m ruff`? Or use in the VS Code extension? Or something else?

---

_Label `question` added by @charliermarsh on 2024-01-03 14:40_

---

_Comment by @jenia90 on 2024-01-04 06:24_

> Are you referring here to invocations of `python -m ruff`? Or use in the VS Code extension? Or something else?

I'm referring to `python -m ruff` or `import ruff.__main__`. If we try running `ruff` by either way, it raises an exception:
```FileNotFoundError: %USERPROFILE%\AppData\Roaming\Python\Python37\Scripts\ruff.exe```

---

_Comment by @zanieb on 2024-01-04 06:30_

Where is the binary being stored?

If you're going to be customizing the path to the binary you're probably just better off calling it directly, right?

---

_Comment by @jenia90 on 2024-01-04 06:46_

It's deployed too all developer machines, but to a different folder than the default `../Scripts`.
I do call it directly, I just thought to add a more generic way of doing it, so that we can add an env var to our python dist and have it working without any special wrappers.

---

_Comment by @charliermarsh on 2024-01-29 13:59_

Unfortunately I think I would prefer not to support this. It's straightforward but it feels non-standard and it does open the door to some confusing behaviors (e.g., `python -m ruff` could then resolve to a binary installed in some other virtual environment).

---

_Closed by @charliermarsh on 2024-01-29 13:59_

---
