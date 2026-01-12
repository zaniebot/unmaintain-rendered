```yaml
number: 1335
title: "\"uv venv\" doesn't find python (Linux)"
type: issue
state: closed
author: BWindey
labels:
  - bug
assignees: []
created_at: 2024-02-15T20:53:14Z
updated_at: 2024-02-15T23:53:14Z
url: https://github.com/astral-sh/uv/issues/1335
synced_at: 2026-01-12T15:58:26Z
```

# "uv venv" doesn't find python (Linux)

---

_@BWindey_

When trying out uv, I wanted to try install a package to the global space. Which isn't possible apparently, but ok, let's make a virtual environment like the command suggested to me: `uv venv`
Result:
```
    × Neither `python` nor `python3` are in `PATH`. Is Python installed?
```
Yes, Python is installed. Running `python3 --version` works. `which python3` returns `/usr/bin/python3`. `echo $PATH` returns `...:/usr/bin/:...` so python definitly is in my path. I don't really know what's going wrong here.

I'm on Zorin OS 17 (derivative of Ubuntu), current python version is 3.10.12.

Is there some more information I can give?

---

_Assigned to @zanieb by @zanieb on 2024-02-15 20:59_

---

_Label `bug` added by @zanieb on 2024-02-15 20:59_

---

_Comment by @zanieb on 2024-02-15 20:59_

Thanks for the report! I'll be looking into this. We've never seen this bug so I'll have to do some digging...

---

_Comment by @zanieb on 2024-02-15 21:01_

Can you share the output of `uv venv -v`?

---

_Comment by @zanieb on 2024-02-15 21:42_

Fixed in #1351 

---

_Closed by @zanieb on 2024-02-15 21:42_

---

_Comment by @BWindey on 2024-02-15 23:53_

Downloaded the new version and it works, awesome!

---
