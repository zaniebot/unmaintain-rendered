```yaml
number: 17405
title: Make .local Parameterizable
type: issue
state: open
author: muellert
labels:
  - enhancement
assignees: []
created_at: 2026-01-11T15:12:07Z
updated_at: 2026-01-12T10:34:24Z
url: https://github.com/astral-sh/uv/issues/17405
synced_at: 2026-01-12T11:01:24Z
```

# Make .local Parameterizable

---

_Issue opened by @muellert on 2026-01-11 15:12_

### Summary

There's a way to override the location of the cache, --cache-dir, but there seems to be no such option to override the directory to be used for ~/.local . It would be great to have such an option to make it easier to create read-only deployments, or where ~/.local doesn't have enough space.

### Example

_No response_

---

_Label `enhancement` added by @muellert on 2026-01-11 15:12_

---

_Comment by @konstin on 2026-01-12 10:34_

uv reads the `XDG_` variables, can you use those?

---
