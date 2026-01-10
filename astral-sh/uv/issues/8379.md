```yaml
number: 8379
title: "uv tree --quiet doesn't show the tree"
type: issue
state: closed
author: bluss
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-10-20T08:44:40Z
updated_at: 2024-10-20T23:24:35Z
url: https://github.com/astral-sh/uv/issues/8379
synced_at: 2026-01-10T04:45:10Z
```

# uv tree --quiet doesn't show the tree

---

_Issue opened by @bluss on 2024-10-20 08:44_

Just a thing I noticed, that,

```
uv tree -q
```

has no output. I think it's expected it shows the tree (and no superfluous status output like resolving etc)

uv 0.4.24


---

_Label `bug` added by @zanieb on 2024-10-20 14:08_

---

_Comment by @charliermarsh on 2024-10-20 16:13_

Yeah `uv tree` and `uv pip list` should both show output.

---

_Label `help wanted` added by @charliermarsh on 2024-10-20 16:13_

---

_Closed by @charliermarsh on 2024-10-20 23:24_

---
