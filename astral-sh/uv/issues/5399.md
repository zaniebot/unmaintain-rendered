```yaml
number: 5399
title: uv not installing build dependencies
type: issue
state: closed
author: EdvardsZ
labels: []
assignees: []
created_at: 2024-07-24T09:14:44Z
updated_at: 2024-07-24T09:18:47Z
url: https://github.com/astral-sh/uv/issues/5399
synced_at: 2026-01-10T04:53:49Z
```

# uv not installing build dependencies

---

_Issue opened by @EdvardsZ on 2024-07-24 09:14_

* A minimal code snippet that reproduces the bug :
`uv pip install git+https://github.com/nerfstudio-project/gsplat`
```
  File "<string>", line 152, in <module>
  File "<string>", line 33, in get_extensions
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that git+https://github.com/nerfstudio-project/gsplat depends on torch, but doesn't declare it as a build dependency. If git+https://github.com/nerfstudio-project/gsplat is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

* However when using pip with `pip install git+https://github.com/nerfstudio-project/gsplat`
Everything installs correctly!

Ofcourse when I install torch and use --no-build-isolation then uv installs it as well.

I think that uv fails to install build dependencies from setup.py (install-requires). 

I am python build tool newbie, it could be that I am somehow mistaken




---

_Comment by @konstin on 2024-07-24 09:18_

This is another case of #2252. The difference between pip and uv is intentional, closing this since `--no-build-isolation` solves this for your case.

---

_Closed by @konstin on 2024-07-24 09:18_

---
