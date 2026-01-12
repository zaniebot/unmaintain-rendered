```yaml
number: 13966
title: "`uv remove` mixes dependencies' end-of-line comments"
type: issue
state: open
author: Lenormju
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-11T13:50:55Z
updated_at: 2025-06-11T13:54:44Z
url: https://github.com/astral-sh/uv/issues/13966
synced_at: 2026-01-12T16:01:40Z
```

# `uv remove` mixes dependencies' end-of-line comments

---

_@Lenormju_

### Summary

Using current latest : `uv 0.7.12`

I have this pyproject.toml file :

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "boto3", # this is boto3
    "requests", # this is requests
]
```

When I do `uv remove requests`, then I get :

```toml
[project]
name = "toto"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "boto3", # this is requests
]
```

So it mixes up the comments between the two lines. 

I was expecting only one line removed, but got :

```diff
-    "boto3", # this is boto3
-    "requests", # this is requests
+    "boto3", # this is requests
```

This is probably linked to #8982. Maybe to #9856 too.

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

0.7.12

### Python version

3.13

---

_Label `bug` added by @Lenormju on 2025-06-11 13:50_

---

_Label `help wanted` added by @zanieb on 2025-06-11 13:54_

---

_Comment by @zanieb on 2025-06-11 13:54_

Thanks for the report!

---
