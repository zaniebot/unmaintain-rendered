```yaml
number: 2655
title: "Cannot install CPU version of `torchvision`"
type: issue
state: closed
author: olokobayusuf
labels: []
assignees: []
created_at: 2024-03-25T19:12:16Z
updated_at: 2024-03-25T19:46:25Z
url: https://github.com/astral-sh/uv/issues/2655
synced_at: 2026-01-12T15:58:39Z
```

# Cannot install CPU version of `torchvision`

---

_@olokobayusuf_

This is pretty similar to #1497 , but now that installing `torch` works, installing `torchvision` doesn't. In my Dockerfile, I run the following:
```
#19 [13/17] RUN uv pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
#19 0.437 Resolved 10 packages in 269ms
#19 3.377 Downloaded 8 packages in 2.93s
#19 3.629 Installed 8 packages in 252ms
#19 3.629  + filelock==3.9.0
#19 3.629  + fsspec==2023.4.0
#19 3.629  + jinja2==3.1.2
#19 3.629  + markupsafe==2.1.3
#19 3.629  + networkx==3.2.1
#19 3.629  + torch==2.2.1+cpu
#19 3.629  + torchvision==0.1.6
#19 3.630  - typing-extensions==4.10.0
#19 3.630  + typing-extensions==4.8.0
#19 DONE 5.6s
```
`uv` incorrectly resolves the `torchvision` version to `0.1.6` which is very old. Trying to do `uv pip install torchvision==0.16.1  --index-url https://download.pytorch.org/whl/cpu` or `uv pip install torchvision>0.16  --index-url https://download.pytorch.org/whl/cpu` both fail. The former can't resolve a specific version; whereas the latter incorrectly installs 0.1.6.

> I can't seem to reproduce this on macOS.

---

_Comment by @charliermarsh on 2024-03-25 19:20_

If you want to resolve to a version that includes a local suffix, you need to specify the local suffix in the requirement definition. This is described in detail here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers.

For example:

`uv pip install torch==2.2.1+cpu torchvision==0.16.0+cpu`

---

_Comment by @olokobayusuf on 2024-03-25 19:45_

This worked. Thanks!

---

_Closed by @olokobayusuf on 2024-03-25 19:45_

---

_Comment by @charliermarsh on 2024-03-25 19:46_

Awesome, thanks for following up.

---
