```yaml
number: 10681
title: "`uv`/`pyenv` conflict?"
type: issue
state: closed
author: ego-thales
labels: []
assignees: []
created_at: 2025-01-16T14:32:54Z
updated_at: 2025-01-16T14:42:47Z
url: https://github.com/astral-sh/uv/issues/10681
synced_at: 2026-01-12T16:00:18Z
```

# `uv`/`pyenv` conflict?

---

_@ego-thales_

Hello,

Is the following normal on a computer with both `pyenv` and `uv`? I'm still not sure how `uv pip` works.

```console
user@pc:~/workspace/project$ uv run which python
/home/user/workspace/project/.venv/bin/python
user@pc:~/workspace/project$ uv run which pip
/home/user/.pyenv/shims/pip
```
I would naively expect the second command to return `/home/user/workspace/project/.venv/bin/pip`.

Does that mean that `uv pip install ...` can yield unexpected results?

Thanks in advance!

---

_Comment by @ego-thales on 2025-01-16 14:39_

My bad, I was confused between `uv pip` and the wrong `uv run pip`.

---

_Closed by @ego-thales on 2025-01-16 14:39_

---

_Comment by @ego-thales on 2025-01-16 14:42_

Actually I'm still surprised by this:
```console
user@pc:~/workspace/project$ source .venv/bin/activate
(project) user@pc:~/workspace/project$ which python
/home/user/workspace/project/.venv/bin/python
(project) user@pc:~/workspace/project$ which pip
/usr/bin/pip
```
I woul, again, expect `/home/user/workspace/project/.venv/bin/pip`

---
