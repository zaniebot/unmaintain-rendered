```yaml
number: 14604
title: "`uvx` does not error when `--from <package>` has a `requires-python` range incompatible with `-p`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-07-14T13:14:37Z
updated_at: 2025-07-14T14:07:32Z
url: https://github.com/astral-sh/uv/issues/14604
synced_at: 2026-01-12T16:01:52Z
```

# `uvx` does not error when `--from <package>` has a `requires-python` range incompatible with `-p`

---

_@zanieb_

### Summary

If you do

```
uv init -p 3.13 example
uvx -p 3.9 --from ./example python --version
```

we'll fail to resolve with 3.9 and resolve again with 3.13 automatically.

I think https://github.com/astral-sh/uv/pull/10401 regressed this.

### Platform

n/a

### Version

main

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-07-14 13:14_

---

_Closed by @zanieb on 2025-07-14 14:07_

---
