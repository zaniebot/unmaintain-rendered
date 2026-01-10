```yaml
number: 16073
title: Project is published even when upload check fails due to Unautherized 401
type: issue
state: open
author: brainslush
labels:
  - bug
assignees: []
created_at: 2025-09-30T11:20:36Z
updated_at: 2025-09-30T14:47:21Z
url: https://github.com/astral-sh/uv/issues/16073
synced_at: 2026-01-10T03:23:54Z
```

# Project is published even when upload check fails due to Unautherized 401

---

_Issue opened by @brainslush on 2025-09-30 11:20_

### Summary

If you provide the wrong or forget to set the index credentials (not the publishing credentials), publishing to a secured private index with
`uv publish --index privateindex`
succeeds with a warning.

pyproject.toml
```toml
#...
[[tool.uv.index]]
name = "privateindex"
url = "https://privateindex.org/simple/"
publish-url = "https://privateindex.org/legacy/"
explicit = true
#...
```

Log
```
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
WARN Package not found in the registry; skipping upload check for ....
Uploading ....
```

I would expect this process to fail for any response code other than 404. 
Is this by design?
I would suggest either to fix the behaviour or add an option to the cli which enforces that the check must succeed.

I would be willing to contribute a fix if interested.

### Platform

Ubuntu 24.04

### Version

0.8.22

### Python version

Python 3.12.11

---

_Label `bug` added by @brainslush on 2025-09-30 11:20_

---

_Assigned to @konstin by @zanieb on 2025-09-30 14:47_

---
