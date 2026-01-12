```yaml
number: 8712
title: uv installs mypy but not the bin?
type: issue
state: closed
author: jbcpollak
labels:
  - needs-mre
assignees: []
created_at: 2024-10-30T23:10:07Z
updated_at: 2025-01-25T02:52:37Z
url: https://github.com/astral-sh/uv/issues/8712
synced_at: 2026-01-12T15:59:32Z
```

# uv installs mypy but not the bin?

---

_@jbcpollak_

In a linux devcontainer using uv `0.4.28`, when I run:

```
uv pip install mypy
```

or I add mypy as a dependency, uv installs it and I can import it from the repl:

```
➜ python
Python 3.10.12 (main, Sep 11 2024, 15:47:36) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import mypy
>>> 
```

but when I run `mypy` on the CLI, bash reports command not found.

When I check in `.venv/bin/` I don't see a mypy executable.

I'm currently working around this with `uv tool install mypy` but I don't think that is the correct solution.

Any advice would be appreciated.


---

_Comment by @zanieb on 2024-10-30 23:20_

I can't reproduce this so we'll need more information

```
❯ uv venv
Using CPython 3.12.7
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install mypy
Resolved 3 packages in 241ms
Prepared 1 package in 773ms
Installed 3 packages in 10ms
 + mypy==1.13.0
 + mypy-extensions==1.0.0
 + typing-extensions==4.12.2
❯ ls .venv/bin | grep mypy
dmypy
mypy
mypyc
```

---

_Label `needs-mre` added by @zanieb on 2024-10-30 23:20_

---

_Comment by @zanieb on 2024-10-30 23:20_

Verbose install logs might be a good start.

---

_Comment by @charliermarsh on 2025-01-25 02:52_

Closing due to lack of follow-up.

---

_Closed by @charliermarsh on 2025-01-25 02:52_

---
