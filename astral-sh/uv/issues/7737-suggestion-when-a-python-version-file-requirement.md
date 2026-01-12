```yaml
number: 7737
title: "Suggestion when a `.python-version` file requirement is incompatible with the project"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2024-09-27T13:06:54Z
updated_at: 2025-02-03T21:52:18Z
url: https://github.com/astral-sh/uv/issues/7737
synced_at: 2026-01-12T15:59:16Z
```

# Suggestion when a `.python-version` file requirement is incompatible with the project

---

_@zanieb_

We should suggest _changing_ the pin, right now we just say it's incompatible:

```
❯ echo "3.9" > .python-version
❯ uv run python
Using CPython 3.9.18
error: The Python request from `.python-version` resolved to Python 3.9.18, which is incompatible with the project's Python requirement: `>=3.12`
```

---

_Label `error messages` added by @zanieb on 2024-09-27 13:06_

---

_Comment by @ugochukwu-850 on 2024-09-27 13:38_

This looks simple , would it be a good first issue 

---

_Comment by @zanieb on 2024-09-27 14:35_

Probably yes.

---

_Label `good first issue` added by @zanieb on 2024-09-27 14:35_

---

_Comment by @SummerGram on 2024-09-28 04:01_

Actually I asked you the same issue in the discord.

Mind if I work on it?

---

_Comment by @mstepan on 2024-10-03 12:40_

This issue PR: https://github.com/astral-sh/uv/pull/7893

The final error message will look like:
```
error: The Python request from `.python-version` resolved to Python 3.9.6, which is incompatible with the project's Python requirement: `>=3.12`. Use `uv python pin`
```

---

_Comment by @jvestell on 2024-11-12 03:21_

Perhaps this issue can be closed. I went through @mstepan's PR #7893 and it looks like it's handled. 

---

_Comment by @edmorley on 2025-02-03 21:50_

This can be closed, since it was implemented via #9590 (which was a copy of #7893).

---

_Closed by @charliermarsh on 2025-02-03 21:52_

---
