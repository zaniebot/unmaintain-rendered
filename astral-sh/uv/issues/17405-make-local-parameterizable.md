```yaml
number: 17405
title: Make .local Parameterizable
type: issue
state: open
author: muellert
labels:
  - question
assignees: []
created_at: 2026-01-11T15:12:07Z
updated_at: 2026-01-12T23:58:27Z
url: https://github.com/astral-sh/uv/issues/17405
synced_at: 2026-01-13T00:22:50Z
```

# Make .local Parameterizable

---

_@muellert_

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

_Label `enhancement` removed by @zanieb on 2026-01-12 23:58_

---

_Label `question` added by @zanieb on 2026-01-12 23:58_

---

_Comment by @zanieb on 2026-01-12 23:58_

Yeah using `XDG_*` is the appropriate pattern here.

---
