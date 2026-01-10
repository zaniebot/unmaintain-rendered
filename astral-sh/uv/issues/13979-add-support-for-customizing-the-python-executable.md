---
number: 13979
title: "Add support for customizing the Python executable directory with `uv python install --bin-dir`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-06-11T22:31:50Z
updated_at: 2025-06-11T22:31:50Z
url: https://github.com/astral-sh/uv/issues/13979
synced_at: 2026-01-10T01:25:41Z
---

# Add support for customizing the Python executable directory with `uv python install --bin-dir`

---

_Issue opened by @zanieb on 2025-06-11 22:31_

### Summary

This occurred to me as a corollary of `--install-dir` when reading https://github.com/astral-sh/uv/issues/13977 and would help with discoverability.

I'm not sure if we'd uninstall these in the future. We'd need to track them, which seems a bit tricky. I think that can be deferred though.

### Example

```
$ uv python install --bin-dir /usr/local/bin
```



---

_Label `enhancement` added by @zanieb on 2025-06-11 22:31_

---
