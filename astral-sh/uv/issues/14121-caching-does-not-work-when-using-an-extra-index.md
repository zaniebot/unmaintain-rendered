---
number: 14121
title: Caching does not work when using an extra index
type: issue
state: closed
author: pschmutz
labels:
  - bug
assignees: []
created_at: 2025-06-17T23:23:16Z
updated_at: 2025-06-20T20:07:32Z
url: https://github.com/astral-sh/uv/issues/14121
synced_at: 2026-01-10T01:25:42Z
---

# Caching does not work when using an extra index

---

_Issue opened by @pschmutz on 2025-06-17 23:23_

### Summary

I have the following `pyproject.toml`

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = []

[[tool.uv.index]]
url = "https://pypi.nvidia.com"

```

If I then execute

```bash
uv add nvidia-nvjitlink-cu12
uv remove nvidia-nvjitlink-cu12
uv add nvidia-nvjitlink-cu12
```

The package is downloaded twice. I would expect that it is found in the cache and not downloaded. Please note that this happens with several other packages as well, I just picked a smaller one to reproduce this. I originally found this when installing 

```bash
uv add 'isaacsim[all,extscache]==4.5.0'
```

I attached the verbose output of the commands listed above.

[uv_output.txt](https://github.com/user-attachments/files/20785422/uv_output.txt)

### Platform

Ubuntu 24.04

### Version

uv 0.7.13

### Python version

Python 3.10.0

---

_Label `bug` added by @pschmutz on 2025-06-17 23:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-17 23:27_

---

_Comment by @charliermarsh on 2025-06-17 23:27_

Lemme take a look...

---

_Comment by @charliermarsh on 2025-06-18 00:01_

Ah okay, it's because they have a Cache-Control header on the wheels:

```
cache-control: max-age=0, no-cache, no-store
```

So we don't cache them.

There's an open issue to allow users to override this (#10444) which could solve this.

---

_Comment by @pschmutz on 2025-06-18 00:05_

Thanks for the quick feedback! I'll follow the open issue you mentioned

---

_Closed by @charliermarsh on 2025-06-20 20:07_

---
