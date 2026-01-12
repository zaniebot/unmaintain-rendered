```yaml
number: 1441
title: Make hashes optional in HTML entries
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-02-16T06:15:17Z
updated_at: 2024-02-16T06:42:23Z
url: https://github.com/astral-sh/uv/issues/1441
synced_at: 2026-01-12T15:58:27Z
```

# Make hashes optional in HTML entries

---

_@charliermarsh_

i'm running devpi and don't have fragments at all which results in

```
 uv pip install --extra-index-url <devpi> <package>
error: Received some unexpected HTML from <devpi>/<package>
  Caused by: Unexpected fragment (expected `#sha256=...`) on URL:
```

(not truncated, there is nothing after `URL:`)

_Originally posted by @davidszotten in https://github.com/astral-sh/uv/issues/1338#issuecomment-1947435048_
            

---

_Label `bug` added by @charliermarsh on 2024-02-16 06:15_

---

_Closed by @charliermarsh on 2024-02-16 06:42_

---
