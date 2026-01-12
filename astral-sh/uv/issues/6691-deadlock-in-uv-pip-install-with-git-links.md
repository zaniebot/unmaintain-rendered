```yaml
number: 6691
title: "Deadlock in `uv pip install` with git links"
type: issue
state: closed
author: gozdal
labels:
  - bug
  - cache
assignees: []
created_at: 2024-08-27T15:07:53Z
updated_at: 2024-10-28T09:16:36Z
url: https://github.com/astral-sh/uv/issues/6691
synced_at: 2026-01-12T15:59:06Z
```

# Deadlock in `uv pip install` with git links

---

_@gozdal_

It seems that `uv pip install` deadlocks when running in parallel installing from git links.

`uv 0.3.3`

`ps auxwww | grep uv`
```
jenkins    44119  0.0  0.0 3430056 35320 ?       Sl   09:06   0:01 uv pip install --compile --upgrade -e starfish/agent[dev] -e starfish/redash[dev] -e starfish/jenkins[dev] -e starfish/client[dev] -e starfish/tests[dev] -e starfish/scripts[dev] -e starfish/starfish[dev] -e starfish/examples[dev]
jenkins    45341  0.0  0.0 3350176 35344 ?       Sl   09:07   0:01 uv pip install --compile --upgrade -e starfish/agent[dev] -e starfish/redash[dev] -e starfish/jenkins[dev] -e starfish/client[dev] -e starfish/tests[dev] -e starfish/scripts[dev] -e starfish/starfish[dev] -e starfish/examples[dev]
```

The editables contain some requirements from git SHA1 hash.

Current time is after 10:00, so the processes are hanging there for more than an hour.

`44119` locked `/home/jenkins/.cache/uv/git-v0/locks/8a439ba06638ac5d` and is waiting for `a172a1434ad2fe00`:

`lsof -p 44119 | grep locks`
```
uv      44119 jenkins   10wW     REG              259,2        0 172099117 /home/jenkins/.cache/uv/git-v0/locks/8a439ba06638ac5d
uv      44119 jenkins   11w      REG              259,2        0 172100733 /home/jenkins/.cache/uv/git-v0/locks/a172a1434ad2fe00
```

OTOH `45341` has `a172a1434ad2fe00` locked and is waiting on `8a439ba06638ac5d`:

`lsof -p 45341 | grep locks`
```
uv      45341 jenkins   10wW     REG              259,2        0 172100733 /home/jenkins/.cache/uv/git-v0/locks/a172a1434ad2fe00
uv      45341 jenkins   11w      REG              259,2        0 172099117 /home/jenkins/.cache/uv/git-v0/locks/8a439ba06638ac5d
```


---

_Label `bug` added by @zanieb on 2024-08-27 15:13_

---

_Label `cache` added by @zanieb on 2024-08-27 15:13_

---

_Comment by @zanieb on 2024-08-27 15:14_

Thanks for the report!

---

_Comment by @charliermarsh on 2024-08-28 17:27_

Just curious, are there any duplicate repositories here? E.g., two requirements from the same repository, but with a different `#subdirectory`?

---

_Comment by @gozdal on 2024-08-28 18:06_

We're not using `#subdirectory`, but git repositories are duplicated between projects, yes.
Here is a reproducer:
```
proj1
proj1/pyproject.toml
proj1/src
proj1/src/__init__.py
proj2
proj2/pyproject.toml
proj2/src
proj2/src/__init__.py
```

`pyproject.toml` (for `proj2`, `proj1` is the same, but with name/description changed)
```toml
[build-system]
requires = ["setuptools>=64"]
build-backend = "setuptools.build_meta"

[project]
requires-python = ">=3.12"
name = "proj2"
description = "Project 2"
version = "0.0.1"
dependencies = [
  "gunicorn @ git+https://github.com/StarfishStorage/gunicorn.git@336276ddb46cd8dd88ebf0f3f21708aa111c06c9 ; sys_platform != 'win32'",
  "python-swiftclient @ git+https://github.com/StarfishStorage/python-swiftclient.git@fbdac94cf75e2fb5d3cf20207d6883fc6bd280b4",
]
```

and then `deadlock.sh`:
```bash
#!/bin/bash

set -euo pipefail

while true; do
  rm -rf .venv1 .venv2
  uv venv .venv1
  uv venv .venv2

  uv pip install --python .venv1 -e proj1 -e proj2 &
  uv pip install --python .venv2 -e proj1 -e proj2 &
  wait
done
```

---

_Comment by @charliermarsh on 2024-08-28 18:13_

Great, thanks!

---

_Comment by @charliermarsh on 2024-08-28 18:27_

Easily reproduced with the above script.

---

_Comment by @zanieb on 2024-08-28 22:21_

I found the bug using your reproduction! Thanks. We'll investigate a fix...

---

_Assigned to @zanieb by @zanieb on 2024-08-28 22:21_

---

_Closed by @zanieb on 2024-08-29 16:16_

---

_Closed by @zanieb on 2024-08-29 16:16_

---

_Comment by @zanieb on 2024-08-29 16:16_

Thanks again for putting together a reproduction!

---

_Comment by @gozdal on 2024-08-30 11:39_

Thanks for fixing it so quickly. I ❤️ uv.

---
