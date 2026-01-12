```yaml
number: 9476
title: unable to uv add httpx or uv pip install httpx on windows
type: issue
state: closed
author: zkurtz
labels:
  - question
assignees: []
created_at: 2024-11-27T15:44:20Z
updated_at: 2024-12-16T20:28:34Z
url: https://github.com/astral-sh/uv/issues/9476
synced_at: 2026-01-12T15:59:51Z
```

# unable to uv add httpx or uv pip install httpx on windows

---

_@zkurtz_

I always get an error like `.venv\...\Scripts\httpx.exe: Access is denied. (os error 5)`. The obvious solution is to run as admin, but in my corporate environment this would require involving IT.

It's unclear how much this is a uv issue vs an httpx issue ([cross posted](https://github.com/encode/httpx/discussions/3417)) vs something else. But I'm suspecting uv somehow because plain `pip install httpx` in a standard `python -m venv .venv` environment still works.



---

_Comment by @zanieb on 2024-11-27 15:46_

Can you install other packages?

I don't really know what's going on here, it seems specific to your system. We use `httpx` in a lot of our tests.

---

_Label `question` added by @zanieb on 2024-11-27 15:46_

---

_Comment by @zkurtz on 2024-11-27 15:59_

Yes, I can install many packages, and httpx is the only one where I've encountered the issue.

---

_Comment by @zanieb on 2024-11-27 16:02_

Perhaps your antivirus is flagging the executable for some reason? There's not anything we can do to help with that.

---

_Comment by @zkurtz on 2024-11-27 16:29_

Right, maybe. Another peculiarity here is that if I simply rerun the command (either in the context of `uv add` or `uv sync`), it succeeds. 

---

_Closed by @zkurtz on 2024-11-27 16:29_

---

_Comment by @DarrenPadraigDV on 2024-12-16 20:28_

@zanieb  I get something similar, I suspect it is crowdstrike related.

---
