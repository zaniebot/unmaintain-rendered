```yaml
number: 10122
title: "Allow \"uvx python\" to execute Python like \"uv run python\""
type: issue
state: closed
author: ksamuel
labels:
  - duplicate
assignees: []
created_at: 2024-12-23T15:33:04Z
updated_at: 2024-12-23T19:19:40Z
url: https://github.com/astral-sh/uv/issues/10122
synced_at: 2026-01-12T16:00:06Z
```

# Allow "uvx python" to execute Python like "uv run python"

---

_@ksamuel_

Right now, if you run `uvx python`, it can fail. Even `uvx -p 3.11 python` will fail to start a Python shell on Windows or Ubuntu because the command doesn't map to a python executable.

However, `uv run python` [works](https://github.com/astral-sh/uv/issues/6348), no matter where you execute it.

It's counter-intuitive, one would expect you can run the installed you installed with uv from uvx. 

What's more, it would be a useful feature to be able to do `uvx -p 3.12 --with pendulum python` to test how this lib works in python 3.12 quickly.

Right now, one has to do  `uvx -p 3.12 --with pendulum ipython` which is fine if you are familiar with the Python ecosystem and `uv` limitations, but probably not for beginners, for whom `uv` is the biggest boon.

---

_Comment by @FishAlchemist on 2024-12-23 18:06_

Are you in need of such a function
```
uvx -p 3.12 --from pendulum python
```
https://docs.astral.sh/uv/guides/tools/#commands-with-different-package-names
![Image](https://github.com/user-attachments/assets/e3fdef16-3823-4f48-9b36-5f175647da23)


Edit:
As for ``uvx python``, I believe it's a duplicate of this issue (https://github.com/astral-sh/uv/issues/7430).


---

_Comment by @ksamuel on 2024-12-23 18:32_

Let's close it as a duplicate

---

_Closed by @ksamuel on 2024-12-23 18:32_

---

_Label `duplicate` added by @zanieb on 2024-12-23 19:19_

---
