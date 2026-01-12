```yaml
number: 15550
title: "Allow `uv format` in unmanaged projects"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2025-08-27T14:35:15Z
updated_at: 2025-09-05T18:14:42Z
url: https://github.com/astral-sh/uv/issues/15550
synced_at: 2026-01-12T16:02:12Z
```

# Allow `uv format` in unmanaged projects

---

_@zanieb_

e.g., from the uv repository

```
❯ echo "x   = 1" > foo.py
❯ uv format -- foo.py
warning: `uv format` is experimental and may change without warning. Pass `--preview-features format` to disable this warning.
error: The project is marked as unmanaged: `/Users/zb/workspace/.worktrees/uv/zb-lock-bin-install`
```

We should definitely allow this. I presume it regressed in #15438 or #15440 

---

_Label `bug` added by @zanieb on 2025-08-27 14:35_

---

_Label `preview` added by @zanieb on 2025-08-27 14:35_

---

_Comment by @jorgehermo9 on 2025-08-27 15:48_

Working on it! Didn't expect that regression... I will add a test case to be sure this does not happen again.

---

_Closed by @zanieb on 2025-09-05 18:14_

---
