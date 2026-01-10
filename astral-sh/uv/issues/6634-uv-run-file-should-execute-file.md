---
number: 6634
title: "`uv run file` should execute file"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2024-08-26T00:17:03Z
updated_at: 2024-08-28T13:26:47Z
url: https://github.com/astral-sh/uv/issues/6634
synced_at: 2026-01-10T01:24:03Z
---

# `uv run file` should execute file

---

_Issue opened by @charliermarsh on 2024-08-26 00:17_

Right now, you need `uv run ./file`, but I think `uv run file` should work too? Similar to how `python file` works.

See: https://github.com/astral-sh/uv/issues/6360


---

_Label `cli` added by @charliermarsh on 2024-08-26 00:17_

---

_Comment by @charliermarsh on 2024-08-26 00:19_

\cc @zanieb 

---

_Comment by @zanieb on 2024-08-26 17:08_

I think no, per shell behavior:

```
❯ touch foo
❯ chmod +x foo
❯ foo
zsh: command not found: foo
❯ ./foo
```

I think we're doing the right thing by special-casing `.py` files.

---

_Closed by @zanieb on 2024-08-26 17:08_

---

_Comment by @teroshan on 2024-08-28 13:26_

I was similarly confused by this, so I'll mention this here for future users searching issues for this:

As someone who used `pipx`, I was using `pipx run standalone-script` or `pipx run standalone-script.py` previously, and was looking to do the same with uv.

I also was using `pipx run` in my shebangs in a similar way, but other issues are open regarding this #6360 

---
