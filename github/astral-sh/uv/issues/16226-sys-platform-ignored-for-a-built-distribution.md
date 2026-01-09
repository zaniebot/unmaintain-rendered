---
number: 16226
title: "`sys_platform` ignored for a built distribution `pymeshlab`"
type: issue
state: closed
author: MalcolmMielle
labels:
  - bug
assignees: []
created_at: 2025-10-10T09:38:24Z
updated_at: 2025-10-29T01:02:40Z
url: https://github.com/astral-sh/uv/issues/16226
synced_at: 2026-01-07T13:12:19-06:00
---

# `sys_platform` ignored for a built distribution `pymeshlab`

---

_Issue opened by @MalcolmMielle on 2025-10-10 09:38_

### Summary

For [pymeshlab](https://inspector.pypi.io/project/pymeshlab/) I have a dependency (nerfstudio) that has this condition:

```toml
"pymeshlab>=2022.2.post2; platform_machine != 'arm64' and platform_machine != 'aarch64'",
"pymeshlab<2023.12.post2; sys_platform == 'win32' and platform_machine != 'arm64' and platform_machine != 'aarch64'",
```

However, this is running and blocking on linux as if the`sys_platform`is ignored. Most of the errors I have seen on this where for Source Distributions which doesn't seem to be the case for pymeshlab. Inmz case it fails to resolve for `2022.2.post2` even though I should be using a higher version.

Any idea what would be happening? I suspect it is because nerfstudio built its wheel as `none-any`? It is a wheel built for linux so I am still a bit confused why that would fail


### Platform

Ubuntu 24.05 - Linux 6.14.0-33-generic x86_64 GNU/Linux

### Version

uv 0.7.13

### Python version

3.12

---

_Label `bug` added by @MalcolmMielle on 2025-10-10 09:38_

---

_Comment by @charliermarsh on 2025-10-29 01:02_

My guess is that this is related to uv's creation of a universal lockfile? To create a `uv.lock`, we need to resolve all dependencies, even those that won't be installed on the current platform; in this case, we need the dependencies for both versions. You can try to solve this with:

```toml
[tool.uv]
environments = ["sys_platform == 'linux'"]
```

That will avoid including the Windows branch in your `uv.lock`.


---

_Closed by @charliermarsh on 2025-10-29 01:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-10-29 01:02_

---
