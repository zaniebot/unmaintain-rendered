---
number: 5858
title: "`uv add https://.../foo.git` throws a confusing error"
type: issue
state: closed
author: charliermarsh
labels:
  - help wanted
  - error messages
  - preview
assignees: []
created_at: 2024-08-07T13:13:36Z
updated_at: 2024-08-09T01:39:49Z
url: https://github.com/astral-sh/uv/issues/5858
synced_at: 2026-01-10T01:23:53Z
---

# `uv add https://.../foo.git` throws a confusing error

---

_Issue opened by @charliermarsh on 2024-08-07 13:13_

We interpret it as a direct URL (e.g., a path to a wheel), and then `.git` doesn't match `.whl` or `.tar.gz` etc.

```
❯ uv add https://github.com/ElmerCSC/elmer_circuitbuilder.git
warning: `uv add` is experimental and may change without warning
Using Python 3.12.4
Creating virtualenv at: .venv
error: Failed to extract archive
  Caused by: Unsupported archive type: elmer_circuitbuilder.git
```


---

_Label `error messages` added by @charliermarsh on 2024-08-07 13:13_

---

_Comment by @charliermarsh on 2024-08-07 13:13_

Arguably this should "just work"?

---

_Comment by @charliermarsh on 2024-08-07 13:14_

(Instead, you need to do, e.g., ` add git+https://github.com/ElmerCSC/elmer_circuitbuilder.git`.)

---

_Comment by @zanieb on 2024-08-07 13:15_

Yeah the `git+` seems overkill to require, let's special case `.git`.

---

_Label `help wanted` added by @charliermarsh on 2024-08-07 13:16_

---

_Label `preview` added by @charliermarsh on 2024-08-07 13:16_

---

_Comment by @charliermarsh on 2024-08-07 13:16_

(I also want to improve that error message to list the supported extensions, it would be much clearer.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-07 15:06_

---

_Referenced in [astral-sh/uv#5866](../../astral-sh/uv/issues/5866.md) on 2024-08-07 15:10_

---

_Referenced in [astral-sh/uv#5888](../../astral-sh/uv/pulls/5888.md) on 2024-08-07 19:37_

---

_Closed by @charliermarsh on 2024-08-09 01:39_

---

_Closed by @charliermarsh on 2024-08-09 01:39_

---
