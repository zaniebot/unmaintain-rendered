---
number: 11825
title: "uv venv doesn't find requires-python for python interpreter with an upper limit"
type: issue
state: closed
author: toppk
labels:
  - bug
assignees: []
created_at: 2025-02-27T07:53:55Z
updated_at: 2025-03-04T23:29:41Z
url: https://github.com/astral-sh/uv/issues/11825
synced_at: 2026-01-10T01:25:11Z
---

# uv venv doesn't find requires-python for python interpreter with an upper limit

---

_Issue opened by @toppk on 2025-02-27 07:53_

### Summary

I have `requires-python = ">=3.12.7,<3.13"` in my pyproject.toml.  and it will not find the right python.  If I switch to just `<3.13` it works.  I've seen some documentation about uv ignoring upper bounds on versioning, which I suppose that is for dependencies, and not the python interpreter selection itself.  


```shell
$  uv python list  --verbose  --python-preference only-system
DEBUG uv 0.6.3
DEBUG Searching for any Python interpreter in search path
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python` (first executable in the search path)
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/usr/bin/python3.12` (search path)
DEBUG Found `cpython-3.8.20-linux-x86_64-gnu` at `/usr/bin/python3.8` (search path)
DEBUG Found `cpython-3.9.21-linux-x86_64-gnu` at `/usr/bin/python3.9` (search path)
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/usr/bin/python3.10` (search path)
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python3.13` (search path)
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/usr/bin/python3.11` (search path)
cpython-3.13.2-linux-x86_64-gnu     /usr/bin/python3.13
cpython-3.13.2-linux-x86_64-gnu     /usr/bin/python3 -> python3.13
cpython-3.13.2-linux-x86_64-gnu     /usr/bin/python -> ./python3
cpython-3.12.9-linux-x86_64-gnu     /usr/bin/python3.12
cpython-3.11.11-linux-x86_64-gnu    /usr/bin/python3.11
cpython-3.10.16-linux-x86_64-gnu    /usr/bin/python3.10
cpython-3.9.21-linux-x86_64-gnu     /usr/bin/python3.9
cpython-3.8.20-linux-x86_64-gnu     /usr/bin/python3.8
```
with `requires-python = ">=3.12.7,<3.13"`
```shell
$  uv venv  --verbose --python-preference only-system
DEBUG uv 0.6.3
DEBUG Found project root: `/home/toppk/myproj`
DEBUG No workspace root found, using project root
DEBUG Using Python request `>=3.12.7, <3.13` from `requires-python` metadata
DEBUG Searching for Python >=3.12.7, <3.13 in search path
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `>=3.12.7, <3.13`
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `>=3.12.7, <3.13`
  × No interpreter found for Python >=3.12.7, <3.13 in search path
```
`requires-python = "<3.13"`
```shell
$  uv venv  --verbose --python-preference only-system
DEBUG uv 0.6.3
DEBUG Found project root: `/home/toppk/myproj`
DEBUG No workspace root found, using project root
DEBUG Using Python request `<3.13` from `requires-python` metadata
DEBUG Searching for Python <3.13 in search path
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `<3.13`
DEBUG Found `cpython-3.13.2-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `<3.13`
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/usr/bin/python3.12` (search path)
Using CPython 3.12.9 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /usr/bin/python3.12
Activate with: source .venv/bin/activate

```

### Platform

fedora 41 Linux 6.12.7-200.fc41.x86_64 x86_64 GNU/Linux

### Version

 uv 0.6.3

### Python version

Python 3.12.9

---

_Label `bug` added by @toppk on 2025-02-27 07:53_

---

_Comment by @charliermarsh on 2025-02-28 00:17_

That's very strange, thanks.

---

_Comment by @zanieb on 2025-02-28 03:12_

Huh weird. I can look into that.

---

_Assigned to @zanieb by @zanieb on 2025-02-28 03:12_

---

_Comment by @toppk on 2025-03-04 07:47_

I tihnk the issue is that python_executables_from_search_path -> find_all_minor -> matches_major_minor

which due to Range, calls: VersionSpecifiers.contains

This is the issue, because does `>=3.12.7,<3.13' contain [3,12] ?   Maybe the fix is to create a wider range for this comparision, or just skip this "optimization 2 for range..."

https://github.com/astral-sh/uv/commit/7beae77283fe49f55c6bfc2f6b3e4d3d82278111

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-04 14:52_

---

_Unassigned @zanieb by @charliermarsh on 2025-03-04 14:52_

---

_Referenced in [astral-sh/uv#11952](../../astral-sh/uv/pulls/11952.md) on 2025-03-04 15:12_

---

_Closed by @charliermarsh on 2025-03-04 23:29_

---

_Closed by @charliermarsh on 2025-03-04 23:29_

---
