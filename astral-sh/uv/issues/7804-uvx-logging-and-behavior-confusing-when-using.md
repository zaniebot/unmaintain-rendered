---
number: 7804
title: "`uvx` logging and behavior confusing when using binary name instead of package name"
type: issue
state: closed
author: ilyagr
labels: []
assignees: []
created_at: 2024-09-30T06:40:33Z
updated_at: 2025-04-30T02:26:37Z
url: https://github.com/astral-sh/uv/issues/7804
synced_at: 2026-01-10T01:24:19Z
---

# `uvx` logging and behavior confusing when using binary name instead of package name

---

_Issue opened by @ilyagr on 2024-09-30 06:40_

I use a binary `px`, but its PyPi package is confusingly called [`pxpx`](https://pypi.org/project/pxpx/). For a clean installation, trying to run it with `uvx` without accounting for this fails as expected:

```console
$ uvx px -v
The executable `px` was not found.
warning: An executable named `px` is not provided by package `px`.
The following executables are provided by `px`:
- hello
```

<details> <summary> Verbose output for reference, showing expected behavior </summary>

```console
$ uvx -v px --version
DEBUG uv 0.4.17
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/ilyagr/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0rc2-macos-aarch64-none`
DEBUG Found `cpython-3.13.0rc2-macos-aarch64-none` at `/Users/ilyagr/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Skipping pre-release cpython-3.13.0rc2-macos-aarch64-none
DEBUG Found managed installation `cpython-3.12.6-macos-aarch64-none`
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/Users/ilyagr/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Acquired lock for `/Users/ilyagr/.local/share/uv/tools`
DEBUG Released lock at `/Users/ilyagr/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `/Users/ilyagr/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: px*
DEBUG Found fresh response for: https://pypi.org/simple/px/
DEBUG Searching for a compatible version of px (*)
DEBUG Selecting: px==0.2.2 [compatible] (px-0.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/79/db/707d9bcca32b58a59b6f7cfc9fcc335355b84c1409be80a3aeb6f51300d1/px-0.2.2-py3-none-any.whl.metadata
DEBUG Tried 1 versions: px 1
DEBUG Split specific environment resolution took 0.001s
Resolved 1 package in 1ms
DEBUG Running `px --version`
DEBUG Looking at `.dist-info` at: /Users/ilyagr/.cache/uv/archive-v0/yI4uQZTVbooEQGyQ6p1Qg/lib/python3.12/site-packages/px-0.2.2.dist-info
DEBUG Looking at `.dist-info` at: /Users/ilyagr/.cache/uv/archive-v0/yI4uQZTVbooEQGyQ6p1Qg/lib/python3.12/site-packages/px-0.2.2.dist-info
The executable `px` was not found.
warning: An executable named `px` is not provided by package `px`.
The following executables are provided by `px`:
- hello
```

</details>

## Problem description

However, let's say we run `uv tool install pxpx` and forget that we did so.

`uvx`'s behavior becomes quite confusing. Note that, even with verbose output, it looks exactly as if the `px` package version 0.2.2 provided the `px` binary that `uvx` ran (this output is nearly identical to the one in the fold above):

```console
$ uvx -v px --version
DEBUG uv 0.4.17
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/ilyagr/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0rc2-macos-aarch64-none`
DEBUG Found `cpython-3.13.0rc2-macos-aarch64-none` at `/Users/ilyagr/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Skipping pre-release cpython-3.13.0rc2-macos-aarch64-none
DEBUG Found managed installation `cpython-3.12.6-macos-aarch64-none`
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/Users/ilyagr/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Acquired lock for `/Users/ilyagr/.local/share/uv/tools`
DEBUG Released lock at `/Users/ilyagr/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `/Users/ilyagr/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: px*
DEBUG Found fresh response for: https://pypi.org/simple/px/
DEBUG Searching for a compatible version of px (*)
DEBUG Selecting: px==0.2.2 [compatible] (px-0.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/79/db/707d9bcca32b58a59b6f7cfc9fcc335355b84c1409be80a3aeb6f51300d1/px-0.2.2-py3-none-any.whl.metadata
DEBUG Tried 1 versions: px 1
DEBUG Split specific environment resolution took 0.000s
Resolved 1 package in 0.75ms
DEBUG Running `px --version`
DEBUG Looking at `.dist-info` at: /Users/ilyagr/.cache/uv/archive-v0/yI4uQZTVbooEQGyQ6p1Qg/lib/python3.12/site-packages/px-0.2.2.dist-info
3.6.5
DEBUG Command exited with code: 0
```

When I looked at [the `px` package](https://pypi.org/project/px/) and realized that this is not supposed to happen, I was quite scared for a hot second that this was a typosquatter that hacked my machine by providing me with a trojan that pretends to be the `px` I expected. But actually, `uvx` just ran the installed version of `px` from `pxpx-3.6.5` without telling me[^1] (not even in verbose logs!).

I think that, ideally, `uvx px` should consistently fail, and perhaps give people a friendly message if it can detect that some of the installed tool on the system provide a `px` binary.

[^1]: Or, maybe, the trojan is so good to take over my web browser and make me believe this is what happened. 😛 

---

_Assigned to @zanieb by @zanieb on 2024-09-30 14:00_

---

_Referenced in [astral-sh/uv#11603](../../astral-sh/uv/pulls/11603.md) on 2025-02-18 19:55_

---

_Closed by @zanieb on 2025-04-29 20:34_

---

_Comment by @ilyagr on 2025-04-30 02:26_

Nice, thank you!

---
