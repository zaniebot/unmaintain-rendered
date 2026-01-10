```yaml
number: 1979
title: "Use full python version in `pyvenv.cfg`"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-26T09:11:12Z
updated_at: 2024-02-26T14:52:22Z
url: https://github.com/astral-sh/uv/pull/1979
synced_at: 2026-01-10T14:54:43Z
```

# Use full python version in `pyvenv.cfg`

---

_Pull request opened by @j178 on 2024-02-26 09:11_

## Summary

For a venv created by `virtualenv`, the `pyvenv.cfg` file specifies the full version string in the `version_info` field:

```
home = /Users/x/.rye/py/cpython@3.12.1/install/bin
implementation = CPython
version_info = 3.12.1.final.0
virtualenv = 20.25.0
include-system-site-packages = false
base-prefix = /Users/x/.rye/py/cpython@3.12.1/install
base-exec-prefix = /Users/x/.rye/py/cpython@3.12.1/install
base-executable = /Users/x/.rye/py/cpython@3.12.1/install/bin/python3
```

The relevant code can be found here: 
https://github.com/pypa/virtualenv/blob/4ca8a20c179a03e649ce696a0df3e6e3344a18c9/src/virtualenv/create/creator.py#L167

This PR changes `pyvenv.cfg` created by uv for better compatibility with `virtualenv`.

## Test Plan

```sh
uv venv
cat .venv/pyvenv.cfg
```


---

_Merged by @charliermarsh on 2024-02-26 14:52_

---

_Closed by @charliermarsh on 2024-02-26 14:52_

---

_Comment by @charliermarsh on 2024-02-26 14:52_

Thanks!

---
