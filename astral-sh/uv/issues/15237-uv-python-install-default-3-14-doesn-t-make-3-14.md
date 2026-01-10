```yaml
number: 15237
title: "`uv python install --default 3.14` doesn't make 3.14 my global python"
type: issue
state: open
author: tdhopper
labels:
  - bug
assignees: []
created_at: 2025-08-12T12:43:36Z
updated_at: 2025-08-12T15:10:11Z
url: https://github.com/astral-sh/uv/issues/15237
synced_at: 2026-01-10T03:32:46Z
```

# `uv python install --default 3.14` doesn't make 3.14 my global python

---

_Issue opened by @tdhopper on 2025-08-12 12:43_

### Summary

I would expect the `--default` flag to work for any version of Python, however, it works for all of them except 3.14. I assume this is related to it being a release candidate, but I get no warning, it just fails silently. 

```shell
bash-5.2$ uv --version
uv 0.8.8 (9a54754b0 2025-08-08)
bash-5.2$ uv python install --default -r 3.14
warning: The `--default` option is experimental and may change without warning. Pass `--preview-features python-install-default` to disable this warning
Installed Python 3.14.0rc1 in 1.67s
 + cpython-3.14.0rc1-macos-aarch64-none (python3.14)
bash-5.2$ python -V
Python 3.12.11
bash-5.2$ uv python install --default -r 3.8
warning: The `--default` option is experimental and may change without warning. Pass `--preview-features python-install-default` to disable this warning
Installed Python 3.8.20 in 1.20s
 ~ cpython-3.8.20-macos-aarch64-none (python, python3, python3.8)
bash-5.2$ python -V
Python 3.8.20
```

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.8 (9a54754b0 2025-08-08)

### Python version

_No response_

---

_Label `bug` added by @tdhopper on 2025-08-12 12:43_

---

_Assigned to @zanieb by @zanieb on 2025-08-12 12:57_

---

_Comment by @zanieb on 2025-08-12 12:57_

Thanks!

---

_Comment by @tdhopper on 2025-08-12 15:10_

@zanieb I took an AI assisted crack at this for my own curiosity. Feel free to use if it's helpful or ignore otherwise.

---
