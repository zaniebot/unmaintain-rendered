```yaml
number: 15839
title: "Add a command to list uv's storage directories"
type: issue
state: open
author: zanieb
labels:
  - needs-design
  - needs-decision
  - cli
assignees: []
created_at: 2025-09-14T14:12:05Z
updated_at: 2025-09-14T14:13:15Z
url: https://github.com/astral-sh/uv/issues/15839
synced_at: 2026-01-12T16:02:18Z
```

# Add a command to list uv's storage directories

---

_@zanieb_

Originally requested in https://github.com/astral-sh/uv/issues/11360

e.g.,

```
$ uv dirs
Python install    /Users/zb/.local/share/uv/python
Python bin        /Users/zb/.local/bin
...
```

---

_Label `needs-design` added by @zanieb on 2025-09-14 14:12_

---

_Label `needs-decision` added by @zanieb on 2025-09-14 14:12_

---

_Label `cli` added by @zanieb on 2025-09-14 14:12_

---

_Comment by @zanieb on 2025-09-14 14:13_

I actually don't think we should add a dedicated CLI command for this, it doesn't seem worth introducing more interface. I'm curious if it's something that more people would find useful though.

---
