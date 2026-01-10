```yaml
number: 2311
title: Store choco cache in GitHub Actions cache
type: pull_request
state: closed
author: zanieb
labels:
  - test-extra
assignees: []
draft: true
base: zb/system-install-build
head: zb/system-install-choco
created_at: 2024-03-09T00:14:07Z
updated_at: 2024-03-09T00:30:07Z
url: https://github.com/astral-sh/uv/pull/2311
synced_at: 2026-01-10T14:54:43Z
```

# Store choco cache in GitHub Actions cache

---

_Pull request opened by @zanieb on 2024-03-09 00:14_

This step is weirdly slow, perhaps it will go faster if we stash the download in GitHub.

Related unresolved issues in choco do not bode well:

- https://github.com/chocolatey/choco/issues/2015
- https://github.com/chocolatey/choco/issues/1390
- https://github.com/chocolatey/choco/issues/2134


---

_Label `test-extra` added by @zanieb on 2024-03-09 00:14_

---

_Comment by @zanieb on 2024-03-09 00:30_

No dice — hope they add support for this upstream.

---

_Closed by @zanieb on 2024-03-09 00:30_

---
