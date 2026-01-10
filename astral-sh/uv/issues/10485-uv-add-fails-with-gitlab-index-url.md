---
number: 10485
title: uv add fails with gitlab index url
type: issue
state: closed
author: npaolini2634
labels:
  - question
assignees: []
created_at: 2025-01-10T23:54:00Z
updated_at: 2025-01-11T04:01:53Z
url: https://github.com/astral-sh/uv/issues/10485
synced_at: 2026-01-10T01:24:54Z
---

# uv add fails with gitlab index url

---

_Issue opened by @npaolini2634 on 2025-01-10 23:54_

Hi, I love using UV, but I'm struggling with switching from uv pip to uv projects.


My repo depends on one package hosted on gitlab. Everything works fine when I use uv pip:
`uv pip install package_name --index-url https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple`

But fails if I try:
`uv add package_name --index-url https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple`

I've also tried --default-index instead of --index-url, unsafe-best-match index-strategy, and a variety of combinations of specifying the source/index in pyproject.toml with no success.

```
Updating https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple (HEAD)
  × Failed to download and build `package_name @ git+https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple`
  ├─ Git operation failed
  ├─ failed to fetch into: C:\Users\user\AppData\Local\uv\cache\git-v0\db\99bb7ff362aa16f5
  ╰─ process didn't exit successfully: `C:\Program Files\Git\cmd\git.exe fetch --force --update-head-ok
      https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple +HEAD:refs/remotes/origin/HEAD` (exit code: 128)
      --- stderr
      remote: 404 Not Found
      fatal: repository 'https://gitlab.XXX.com/api/v4/projects/519/packages/pypi/simple/' not found
```

The lack of .pypirc support is also making things difficult for publishing, but I know that's a separate issue.

Window 11, UV 0.5.17

Thanks!


---

_Comment by @zanieb on 2025-01-11 00:04_

It's really weird that we're trying to fetch the dependency with `git`.

Can you share a simple `pyproject.toml` that reproduces the issue? Do you have a `tool.uv.sources` entry for that package?

---

_Comment by @npaolini2634 on 2025-01-11 00:49_

Ahh sorry, I think I was confused by an example here:
https://docs.astral.sh/uv/concepts/projects/dependencies/#adding-dependencies

and had a git source reference in pyproject.toml that shouldn't have been there. Working now.

Thanks for the quick response though!

---

_Comment by @zanieb on 2025-01-11 04:01_

Good to hear!

---

_Closed by @zanieb on 2025-01-11 04:01_

---

_Label `question` added by @zanieb on 2025-01-11 04:01_

---
