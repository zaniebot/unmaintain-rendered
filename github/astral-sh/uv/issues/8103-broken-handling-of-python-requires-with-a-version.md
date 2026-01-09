---
number: 8103
title: "Broken handling of `python-requires` with a `!=` version specification"
type: issue
state: closed
author: scolby33
labels: []
assignees: []
created_at: 2024-10-10T18:27:28Z
updated_at: 2024-10-10T19:03:28Z
url: https://github.com/astral-sh/uv/issues/8103
synced_at: 2026-01-07T13:12:17-06:00
---

# Broken handling of `python-requires` with a `!=` version specification

---

_Issue opened by @scolby33 on 2024-10-10 18:27_

If you create a directory containing only the following `pyproject.toml`:
```toml
[project]
name = "broken_requires"
version = "0.1.0"
requires-python = "!=3.11"
```

And then run `uv build`, you receive the following error:
```console
$ uv build --verbose
DEBUG uv 0.4.20 (Homebrew 2024-10-08)
DEBUG Found workspace root: `/Users/scott/src/broken_requires`
DEBUG Adding current workspace member: `/Users/scott/src/broken_requires`
DEBUG Searching for Python <3.11, >3.11 in managed installations or system path
DEBUG Searching for managed installations at `/Users/scott/.local/share/uv/python`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/Users/scott/.local/share/uv/python`
DEBUG Released lock at `/Users/scott/.local/share/uv/python/.lock`
error: No interpreter found for Python <3.11, >3.11 in virtual environments, managed installations, or system path
```

It seems to me that uv is expanding the `!=3.11` version specification to `<3.11, >3.11`, which is unfortunately impossible to satisfy. (See [here](https://packaging.python.org/en/latest/specifications/version-specifiers/#id5): 'The comma (“,”) is equivalent to a logical **and** operator')

I think uv should have an internal representation of a logical **or** operator (which is not part of the version specifiers scheme) or should not perform the improper expansion of a **not equal** to a logical **and**.

I am running uv installed from Homebrew on macOS 14.6.1.

```console
$ uv --version
uv 0.4.20 (Homebrew 2024-10-08)
```

---

_Comment by @zanieb on 2024-10-10 18:34_

Is this a duplicate of #7862 which was recently resolved by https://github.com/astral-sh/uv/pull/7897 (not yet released)

---

_Comment by @charliermarsh on 2024-10-10 18:45_

Yeah it is 

---

_Comment by @scolby33 on 2024-10-10 18:51_

Oh thanks for finding that! I tried to search but I guess GitHub didn't like the `!=` string in the search query.

---

_Closed by @scolby33 on 2024-10-10 18:51_

---

_Comment by @charliermarsh on 2024-10-10 19:03_

No problem, that's a brutal search query :D

---
