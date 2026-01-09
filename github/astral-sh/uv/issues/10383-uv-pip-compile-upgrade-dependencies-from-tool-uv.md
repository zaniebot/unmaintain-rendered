---
number: 10383
title: uv pip-compile upgrade dependencies from tool.uv.sources even without the --upgrade flag
type: issue
state: closed
author: Corentin-Bravo
labels:
  - bug
assignees: []
created_at: 2025-01-08T08:25:14Z
updated_at: 2025-01-16T18:10:01Z
url: https://github.com/astral-sh/uv/issues/10383
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip-compile upgrade dependencies from tool.uv.sources even without the --upgrade flag

---

_Issue opened by @Corentin-Bravo on 2025-01-08 08:25_

- Version of uv : 0.5.15
- OS: Ubuntu 22.04.4 LTS 
- Command ran: `uv pip compile pyproject.toml --no-emit-index-url --python-platform=linux --output-file=requirements.txt`
- (reduced and curated) Content of my pyproject.toml: 
```
dependencies = [
  "myPrivateLibrary_1~=84.0,>=84.0.1",
  "myPrivateLibrary_2~=6.3,>=6.3.3",
]
[[tool.uv.index]]
name = "myIndex"
url = "myIndexUrl"

[tool.uv.sources]
myPrivateLibrary_1 = {index = "myIndex"}
myPrivateLibrary_2 = {index = "myIndex"}
```
- Original content of my requirements.txt:
```
myPrivateLibrary_2==6.4.1
    # via myProject (pyproject.toml)
myPrivateLibrary_1==84.0.0
    # via myProject (pyproject.toml)
``` 

- Expected content of my requirements.txt after running the command:
```
myPrivateLibrary_2==6.4.1
    # via myProject (pyproject.toml)
myPrivateLibrary_1==84.0.1
    # via myProject (pyproject.toml)
``` 

- Actual content  of my requirements.txt after running the command:
```
myPrivateLibrary_2==6.5.0
    # via myProject (pyproject.toml)
myPrivateLibrary_1==84.0.1
    # via myProject (pyproject.toml)
``` 

Note that removing the `tool.uv.sources` part of the pyproject.toml and running the same command produce the expected behaviour.

It seems as though I ran with the `--upgrade-package` flag on all packages in tool.uv.sources, which does not seem to be documented, intended or logical from an end user point of view.

If the observed behaviour is intended for your side, is there a way to circumvent it (other than removing the `tool.uv.sources` block, as I want to make use of the "explicit" index feature) ? Could you document this behaviour, or point me towards the relevant documentation if it exists ?

Thank you in advance


---

_Renamed from "uv pip-compile upgrade dependencies from extra-index even without the --upgrade flag" to "uv pip-compile upgrade dependencies from tool.uv.sources even without the --upgrade flag" by @Corentin-Bravo on 2025-01-08 08:34_

---

_Comment by @charliermarsh on 2025-01-13 02:44_

Thanks. Can you put together a minimal Git repo that I can clone to reproduce this? I need to make sure that it's reproducible and that I take the exact same steps as you've done here.

---

_Label `needs-mre` added by @charliermarsh on 2025-01-13 02:44_

---

_Comment by @Corentin-Bravo on 2025-01-16 14:07_

Sure,
I believe that's as minimal as it gets:  https://github.com/Corentin-Bravo/mre

Note that I used the public PyPi as index in this MRE to make it easier to run.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-16 15:10_

---

_Label `needs-mre` removed by @charliermarsh on 2025-01-16 15:34_

---

_Label `bug` added by @charliermarsh on 2025-01-16 15:34_

---

_Comment by @charliermarsh on 2025-01-16 15:34_

Thanks!

---

_Referenced in [astral-sh/uv#10690](../../astral-sh/uv/pulls/10690.md) on 2025-01-16 17:44_

---

_Closed by @charliermarsh on 2025-01-16 18:10_

---

_Referenced in [astral-sh/uv#10782](../../astral-sh/uv/pulls/10782.md) on 2025-01-20 17:12_

---
