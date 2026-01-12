```yaml
number: 6168
title: "uv misinterprets `python_version in \"2.6 2.7 3.2 3.3\"` marker"
type: issue
state: closed
author: sbidoul
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-08-17T12:15:38Z
updated_at: 2024-08-17T16:12:29Z
url: https://github.com/astral-sh/uv/issues/6168
synced_at: 2026-01-12T15:59:01Z
```

# uv misinterprets `python_version in "2.6 2.7 3.2 3.3"` marker

---

_@sbidoul_

It seems uv 0.2.37 misinterprets this marker:  `python_version in "2.6 2.7 3.2 3.3"`.

How to reproduce:

`uv pip install pickleshare` installs `pathlib2`, which is declared as `Requires-Dist: pathlib2; python_version in "2.6 2.7 3.2 3.3"` in the `pickleshare` wheel METADATA.

---

_Label `bug` added by @charliermarsh on 2024-08-17 15:54_

---

_Label `compatibility` added by @charliermarsh on 2024-08-17 15:54_

---

_Comment by @charliermarsh on 2024-08-17 15:54_

Yeah this is known. We should fix it. There’s another issue somewhere.

---

_Comment by @zanieb on 2024-08-17 15:57_

See https://github.com/astral-sh/uv/issues/3683

---

_Closed by @sbidoul on 2024-08-17 16:12_

---
