---
number: 9132
title: "`ruff format --check` outputs \"files left unchanged\", which is slightly confusing"
type: issue
state: closed
author: johnthagen
labels:
  - good first issue
  - cli
assignees: []
created_at: 2023-12-14T16:59:51Z
updated_at: 2024-11-06T12:32:24Z
url: https://github.com/astral-sh/ruff/issues/9132
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff format --check` outputs "files left unchanged", which is slightly confusing

---

_Issue opened by @johnthagen on 2023-12-14 16:59_

This is pretty minor, but when you run:

```
$ ruff format --check .
401 files left unchanged
```

It's slightly confusing to me that it says "files left unchanged", since in `--check` mode, it will _never_ change any files. The first time I saw this, I thought I had accidentally forgotten to add the `--check` argument in a Nox session.

Would it be a small improvement if `ruff format` checked if it was in `--check` mode and changed the output to something like:

```
$ ruff format --check .
401 files checked successfully
```

```
$ ruff format --check .
Would reformat: main.py
1 file needs to be reformatted, 400 files checked successfully
```

Then again, maybe using the exact output from normal `ruff format` is better for consistency? No strong opinions from me, just an observation. Feel free to close if you disagree.

---

_Comment by @zanieb on 2023-12-14 17:47_

This makes sense to me! Thanks for raising.

---

_Label `good first issue` added by @zanieb on 2023-12-14 17:47_

---

_Label `cli` added by @zanieb on 2023-12-14 17:47_

---

_Comment by @charliermarsh on 2023-12-15 00:13_

Good call. Or "already formatted"? (Is that less clear?)

---

_Comment by @johnthagen on 2023-12-15 11:51_

@charliermarsh I like "already formatted" as well!

---

_Referenced in [astral-sh/ruff#9153](../../astral-sh/ruff/pulls/9153.md) on 2023-12-16 00:13_

---

_Closed by @charliermarsh on 2023-12-18 05:07_

---

_Comment by @Cherrymelon on 2024-11-04 08:40_

in version 0.7.2,  type `ruff format ./dataset/api.py`  still return 1 file left unchanged,
I think it need be update also

---

_Comment by @MichaReiser on 2024-11-06 12:32_

Looking at the PR, this seems intentional. See [this comment](https://github.com/astral-sh/ruff/pull/9153#discussion_r1428808967), although I'm not sure why. @charliermarsh do you remember?

---
