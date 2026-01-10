```yaml
number: 17034
title: "Show a progress bar for hashing large files `uv publish`"
type: issue
state: open
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2025-12-08T17:23:33Z
updated_at: 2025-12-08T19:50:14Z
url: https://github.com/astral-sh/uv/issues/17034
synced_at: 2026-01-10T03:11:35Z
```

# Show a progress bar for hashing large files `uv publish`

---

_Issue opened by @konstin on 2025-12-08 17:23_

Kinda low-priority since most publishing is from CI and for small files, but it would be nice to show a progress bar that we're hashing a file for upload in `uv publish`, which can take a few seconds for a large files on a slow disk.

---

_Label `enhancement` added by @konstin on 2025-12-08 17:23_

---

_Comment by @yumeminami on 2025-12-08 19:26_

@konstin 

Hi! I implement the feat like below and if you think it is acceptable, I can make a PR.

Hash

```
uv-dev publish --index testpypi          
Publishing 2 files to https://test.pypi.org/legacy/
Uploading git_mcp_server-0.2.7-py3-none-any.whl (95.5MiB)
Hashing git_mcp_server-0.2.7-py3-none-any.whl ------------------------------ 84.77 MiB/95.48 MiB 
```

Uploading

```
Publishing 2 files to https://test.pypi.org/legacy/
Uploading git_mcp_server-0.2.7-py3-none-any.whl (95.5MiB)
Uploading git_mcp_server-0.2.7-py3-none-any.whl ------------------------------ 15.80 MiB/95.48 MiB   
```

---

_Comment by @zanieb on 2025-12-08 19:50_

A pull request sounds good!

---
