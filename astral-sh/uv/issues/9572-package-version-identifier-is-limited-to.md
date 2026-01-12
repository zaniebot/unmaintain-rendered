```yaml
number: 9572
title: Package version identifier is limited to 18446744073709551615
type: issue
state: open
author: sassanh
labels:
  - compatibility
assignees: []
created_at: 2024-12-02T09:26:16Z
updated_at: 2024-12-02T13:35:43Z
url: https://github.com/astral-sh/uv/issues/9572
synced_at: 2026-01-12T15:59:53Z
```

# Package version identifier is limited to 18446744073709551615

---

_@sassanh_

This is a duplicate of https://github.com/astral-sh/uv/issues/8271, the author of that issue closed it as they didn't have that requirement anymore, but the issue is not resolved yet.

We are using big numbers to put readable date + a numeric representation of the commit hash in the dev version identifier. We can reduce the size of hash or sacrifice something to forcefully make it smaller than the limit, but I think things would be easier for people if we can get rid of this limit as the combination of this limit and the limits in PyPI avoiding free use of package versioning makes it hard to publish development builds in CD jobs.

---

_Label `compatibility` added by @charliermarsh on 2024-12-02 13:35_

---
