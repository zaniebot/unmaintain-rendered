---
number: 5645
title: Intermitent installation errors when install from a git ref
type: issue
state: open
author: edgarrmondragon
labels:
  - bug
assignees: []
created_at: 2024-07-31T00:29:55Z
updated_at: 2024-07-31T00:30:53Z
url: https://github.com/astral-sh/uv/issues/5645
synced_at: 2026-01-07T13:12:17-06:00
---

# Intermitent installation errors when install from a git ref

---

_Issue opened by @edgarrmondragon on 2024-07-31 00:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Environment: Official Python 3.10 Docker image `python:3.10-slim` (so, minimal Debian)
uv: 0.2.27 through 0.2.31.

---

When attempting to install `git+https://github.com/meltano/dbt-ext.git@main`, we sometimes see the following errors:

```
error: Git operation failed
  Caused by: failed to create locked file '/root/.cache/uv/git-v0/checkouts/d7d6995f0449e5f3/b39a873/.git/index.lock': No such file or directory; class=Os (2); code=NotFound (-3)
```

```
error: Git operation failed
  Caused by: could not find repository at '/root/.cache/uv/git-v0/checkouts/d7d6995f0449e5f3/b39a873/.git/'; class=Repository (6); code=NotFound (-3)
```

I'm trying to get some MREs for you, but the errors do seem to be rather random and intermittent.

---

_Comment by @charliermarsh on 2024-07-31 00:30_

Very interesting, thank you! Let me see how this _could_ be happening.

---

_Label `bug` added by @charliermarsh on 2024-07-31 00:30_

---
