---
number: 12164
title: "`uv sync --locked` does not detect incorrect `uv.lock`"
type: issue
state: closed
author: Nugine
labels:
  - bug
assignees: []
created_at: 2025-03-14T07:19:42Z
updated_at: 2025-03-17T23:56:09Z
url: https://github.com/astral-sh/uv/issues/12164
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv sync --locked` does not detect incorrect `uv.lock`

---

_Issue opened by @Nugine on 2025-03-14 07:19_

### Summary

Discovered from a bug of dependabot: 
https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2723709984

To reproduce
1. select any package in uv.lock
2. change its version to any valid semver 
    + edit the `package.version` field directly in `uv.lock`
3. run `uv sync --locked`

The installed package is still the previous installed version. It mismatches with what the lock file specified.

There is no warning or error for this case. `uv` continues sliently.

### Platform

Linux 6.8.0-55-generic x86_64 GNU/Linux

### Version

uv 0.6.6

### Python version

Python 3.13.2

---

_Label `bug` added by @Nugine on 2025-03-14 07:19_

---

_Comment by @konstin on 2025-03-14 21:07_

When you say "change its version to any valid semver", do you mean editing the `package.version` field directly in `uv.lock`, without changing the `sdist` or `wheels` entries?

---

_Referenced in [tattle-made/feluda#471](../../tattle-made/feluda/issues/471.md) on 2025-03-14 22:12_

---

_Comment by @shauneccles on 2025-03-14 22:20_

> When you say "change its version to any valid semver", do you mean editing the `package.version` field directly in `uv.lock`, without changing the `sdist` or `wheels` entries?

I think thats what they mean - that is currently what dependabot is doing to update the lockfile - 
https://github.com/LedFx/LedFx/pull/1311/files

![Image](https://github.com/user-attachments/assets/a5c779e9-b1ea-45bd-abce-a20e47371f76)


---

_Referenced in [astral-sh/uv#12235](../../astral-sh/uv/pulls/12235.md) on 2025-03-17 10:10_

---

_Referenced in [astral-sh/uv#12237](../../astral-sh/uv/pulls/12237.md) on 2025-03-17 11:39_

---

_Closed by @zanieb on 2025-03-17 22:33_

---

_Comment by @zanieb on 2025-03-17 23:55_

See also https://github.com/astral-sh/uv/issues/12254

---
