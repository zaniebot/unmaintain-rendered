```yaml
number: 16297
title: Python crash due to stale version
type: issue
state: closed
author: brian316
labels:
  - bug
assignees: []
created_at: 2025-10-14T17:01:56Z
updated_at: 2025-10-15T00:57:13Z
url: https://github.com/astral-sh/uv/issues/16297
synced_at: 2026-01-12T16:02:28Z
```

# Python crash due to stale version

---

_@brian316_

### Summary

## Problem
When i first initialized my project using `uv init` and `uv sync` then i ran python `uv run python` the process would immediately get killed

```shell
cat .python-version
3.11

uv run python -c "print('hello')"

[3]    42262 killed     python -c "print('hello')"
```

My debugging
```shell
xattr -l .venv/bin/python3
which python3
file $(which python3)

com.apple.provenance:
/Users/brian/sandbox/.venv/bin/python3
/Users/brian/sandbox/.venv/bin/python3: Mach-O 64-bit executable arm64
```

I tried to see if i could manually install python again. It installed this version but still got the killed process error
```shell
uv python install 3.11
Installed Python 3.11.12 in 1.17s
 + cpython-3.11.12-macos-aarch64-none (python3.11)

uv run python -c "print('hello')"

[3]    45166 killed     python -c "print('hello')"
```

## Solution

I think somehow the initial python version i had was corrupted, but even then when i tried to install python 3.11 again it only installed `3.11.12` not `3.11.14`

So I fixed by uninstalling (uninstalled multiple versions)
```shell
uv python uninstall 3.11
Searching for Python versions matching: Python 3.11
Uninstalled 2 versions in 301ms
 - cpython-3.11.11-macos-aarch64-none
 - cpython-3.11.12-macos-aarch64-none (python3.11)
```

and running the following installed latest version `3.11.14`
```shell
uv sync
Using CPython 3.11.14
Creating virtual environment at: .venv
...

uv run python -c "print('hello')"

hello
```

## Conclusion

Maybe `uv` should check integrity of python version to reinstall or upgrade minor version? Not sure how it initially got to this state anyhow as other version were working fine for me.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.9.2 (141369ce7 2025-10-10)

### Python version

Python 3.11.14

---

_Label `bug` added by @brian316 on 2025-10-14 17:01_

---

_Comment by @zanieb on 2025-10-14 22:25_

This seems like a pretty hard failure state for us to reliably detect. It seems like the process was just getting killed by the operating system? That could happen for arbitrary reasons, so I'm not sure it should trigger a reinstall or upgrade.

I'm not sure why 3.11.12 -> 3.11.14 fixed this for you.

---

_Comment by @brian316 on 2025-10-14 23:13_

> This seems like a pretty hard failure state for us to reliably detect. It seems like the process was just getting killed by the operating system? That could happen for arbitrary reasons, so I'm not sure it should trigger a reinstall or upgrade.
> 
> I'm not sure why 3.11.12 -> 3.11.14 fixed this for you.

agree not easily debuggable. wish I was able to replicate. wanted to at least write something up in case someone else runs into this issue. 

---

_Closed by @brian316 on 2025-10-14 23:13_

---

_Comment by @zanieb on 2025-10-15 00:57_

Appreciate the report nonetheless, we can always dig deeper if someone else reproduces it.

---
