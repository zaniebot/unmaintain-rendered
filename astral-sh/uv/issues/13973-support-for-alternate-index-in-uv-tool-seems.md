---
number: 13973
title: "Support for alternate index in `uv tool` seems incomplete"
type: issue
state: open
author: ukalwa
labels:
  - question
assignees: []
created_at: 2025-06-11T17:22:52Z
updated_at: 2025-06-11T20:38:12Z
url: https://github.com/astral-sh/uv/issues/13973
synced_at: 2026-01-10T01:25:41Z
---

# Support for alternate index in `uv tool` seems incomplete

---

_Issue opened by @ukalwa on 2025-06-11 17:22_

### Question

Hello,

With `~/.config/uv/uv.toml` as:

```toml
[[index]]
name = "private-pypi"
url = "http://localhost:8080/simple/"
explicit = true
```

I am unable to run cli script `uvx --index private-pypi package_a`. Here are the logs with `uvx -v --index private-pypi package_a`:

```shell
$ uvx -v --index private-pypi package_a
DEBUG uv 0.7.12
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/user/.local/share/uv/python`
DEBUG Found `cpython-3.10.12-linux-aarch64-gnu` at `/usr/bin/python` (first executable in the search path)
DEBUG Acquired lock for `/home/user/.local/share/uv/tools`
DEBUG Checking for Python environment at `/home/user/.local/share/uv/tools/package-a`
DEBUG Released lock at `/home/user/.local/share/uv/tools/.lock`
DEBUG Assessing Python executable as base candidate: /usr/bin/python
DEBUG Caching via base interpreter: `/usr/bin/python`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.12
DEBUG Solving with target Python version: >=3.10.12
DEBUG Adding direct dependency: package-a*
DEBUG No cache entry for: https://pypi.org/simple/package-a/
DEBUG No netrc file found
DEBUG Searching for a compatible version of package-a (*)
DEBUG No compatible version found for: package-a
  × No solution found when resolving tool dependencies:
  ╰─▶ Because package-a was not found in the package registry and you require package-a, we can conclude that your requirements are unsatisfiable.
```

But I can install and run it if I remove `explicit` from index.


### Platform

Linux, amd64

### Version

0.7.12

---

_Label `question` added by @ukalwa on 2025-06-11 17:22_

---

_Renamed from "Support for alternate index for uv tools seems incomplete" to "Support for alternate index in `uv tool` seems incomplete" by @ukalwa on 2025-06-11 17:35_

---

_Comment by @zanieb on 2025-06-11 19:09_

We don't support requesting indexes by name via the `--index` flag yet, though we want to add support.

---

_Comment by @zanieb on 2025-06-11 19:13_

I've opened https://github.com/astral-sh/uv/issues/13974 to track that.

---

_Referenced in [astral-sh/uv#13974](../../astral-sh/uv/issues/13974.md) on 2025-06-11 19:13_

---

_Comment by @ukalwa on 2025-06-11 20:38_

Sounds good. It also looks like sources cannot be added to `uv.toml`. I thought I could force the index by adding `package_a` to the sources table in `uv.toml` but couldn't. Is that also a planned feature?

---
