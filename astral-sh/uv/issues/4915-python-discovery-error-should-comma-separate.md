```yaml
number: 4915
title: Python discovery error should comma-separate
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
created_at: 2024-07-09T05:24:30Z
updated_at: 2024-07-09T05:51:31Z
url: https://github.com/astral-sh/uv/issues/4915
synced_at: 2026-01-12T15:58:52Z
```

# Python discovery error should comma-separate

---

_@charliermarsh_

In:

```
❯ cargo run -- run -vv --preview --isolated --python 3.12.4 python -V
error: No interpreter found for Python 3.12.4 in virtual environments or managed installations or system path
```

It should read: `virtual environments, managed installations, or system path`.


---

_Label `error messages` added by @charliermarsh on 2024-07-09 05:24_

---

_Label `preview` added by @charliermarsh on 2024-07-09 05:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 05:37_

---

_Closed by @charliermarsh on 2024-07-09 05:51_

---
