```yaml
number: 6529
title: "`uv run -` should pipe input"
type: issue
state: open
author: charliermarsh
labels:
  - performance
  - internal
assignees: []
created_at: 2024-08-23T15:50:06Z
updated_at: 2024-08-26T18:40:30Z
url: https://github.com/astral-sh/uv/issues/6529
synced_at: 2026-01-12T15:59:05Z
```

# `uv run -` should pipe input

---

_@charliermarsh_

See: https://github.com/astral-sh/uv/pull/6481#discussion_r1728931167

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 15:50_

---

_Label `internal` added by @charliermarsh on 2024-08-23 15:50_

---

_Label `performance` added by @zanieb on 2024-08-23 15:53_

---

_Comment by @zanieb on 2024-08-23 15:53_

Is this performance? I'm not sure. Memory usage at least.

---

_Comment by @charliermarsh on 2024-08-23 15:55_

Yeah makes sense, thanks.

---

_Comment by @charliermarsh on 2024-08-25 15:58_

I think we can't do this if we want to support `tool.uv` settings in scripts from stdin... Which we probably should? Since, in that case, we need to extract the settings before we run it.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-26 14:43_

---

_Comment by @zanieb on 2024-08-26 16:56_

I think we'd just need to read until we finish the `scripts` block then stream the rest via a pipe — which is a bit more complicated but seems pretty reasonable. I don't think this is high priority until someone encounters a problem with storing the entire file in memory.

---

_Comment by @charliermarsh on 2024-08-26 17:59_

The `scripts` block can be anywhere in the file, so we'd have to read the whole thing in the negative case :joy:

---

_Comment by @zanieb on 2024-08-26 18:40_

It can!? What the heck, spec.

---
