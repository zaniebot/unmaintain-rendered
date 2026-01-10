---
number: 5389
title: "`uv python pin` message \"Updated `.python-version`\" can be misleading"
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-07-24T01:09:22Z
updated_at: 2024-08-20T18:21:32Z
url: https://github.com/astral-sh/uv/issues/5389
synced_at: 2026-01-10T01:23:48Z
---

# `uv python pin` message "Updated `.python-version`" can be misleading

---

_Issue opened by @zanieb on 2024-07-24 01:09_

As in this repository, if there's a `.python-versions` file:

```
❯ rm .python-version
❯ uv python pin pypy@3.11
warning: `uv python pin` is experimental and may change without warning
warning: No interpreter found for PyPy 3.11 in managed installations or system path
Updated `.python-version` from `3.12.1` -> `pypy@3.11`

❯ cat .python-version
pypy@3.11
❯ cat .python-versions
3.12.1
3.11.7
3.10.13
3.9.18
3.8.18
3.8.12
```

Yes the default version changed, but it sounds like there was a `.python-version` file.

---

_Label `cli` added by @zanieb on 2024-07-24 01:10_

---

_Label `preview` added by @zanieb on 2024-07-24 01:10_

---

_Comment by @charliermarsh on 2024-07-24 02:03_

What do you think is the ideal message in that case?

---

_Comment by @zanieb on 2024-07-24 02:05_

I'm not sure, it's annoying. Maybe the problem is that we should actually be inserting an entry into the front of the `.python-versions` file instead of creating a second version file?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---
