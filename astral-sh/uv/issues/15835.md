```yaml
number: 15835
title: "Ban configuration of directories in the `pyproject.toml`"
type: issue
state: open
author: zanieb
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-09-14T14:02:25Z
updated_at: 2025-09-14T14:52:03Z
url: https://github.com/astral-sh/uv/issues/15835
synced_at: 2026-01-10T03:23:54Z
```

# Ban configuration of directories in the `pyproject.toml`

---

_Issue opened by @zanieb on 2025-09-14 14:02_

Right now, the `pyproject.toml` allows setting the `cache-dir` which seems... weird?

I think if we introduce a concept of "trusted projects" it could be okay, but I'd be confused if a project could change uv's storage directories in general.

I think a decision here could be a blocker for adding more directory configuration, e.g.:

- #15834 
- #6506 

---

_Label `configuration` added by @zanieb on 2025-09-14 14:04_

---

_Label `needs-decision` added by @zanieb on 2025-09-14 14:04_

---

_Comment by @Ma-Shell on 2025-09-14 14:44_

> I think a decision here could be a blocker for adding more directory configuration

Both linked issues are about `uv.toml`, whereas the current issue is `pyproject.toml`, so I wouldn't see the decision here as a blocker for the other two, or is there some inherent connection that everything in `uv.toml` must also be exposed to `pyproject.toml`?

---

_Comment by @zanieb on 2025-09-14 14:52_

>  is there some inherent connection that everything in uv.toml must also be exposed to pyproject.toml?

Yeah, I think that's basically what would happen with the way things are is structured right now.

---
