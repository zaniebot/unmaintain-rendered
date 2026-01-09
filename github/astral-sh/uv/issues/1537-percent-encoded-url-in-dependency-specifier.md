---
number: 1537
title: Percent-encoded URL in dependency specifier rejected
type: issue
state: closed
author: layday
labels:
  - bug
assignees: []
created_at: 2024-02-16T19:51:40Z
updated_at: 2024-02-16T21:11:17Z
url: https://github.com/astral-sh/uv/issues/1537
synced_at: 2026-01-07T13:12:16-06:00
---

# Percent-encoded URL in dependency specifier rejected

---

_Issue opened by @layday on 2024-02-16 19:51_

Dependencies of the form:

```
uv pip install 'foo @ file:///a/b/c%2Bd'
```

error with:

```
error: Distribution not found at: [...]
```

These are supported by pip.

---

_Label `bug` added by @charliermarsh on 2024-02-16 20:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 20:38_

---

_Comment by @charliermarsh on 2024-02-16 20:38_

Will fix now.

---

_Referenced in [astral-sh/uv#1541](../../astral-sh/uv/pulls/1541.md) on 2024-02-16 20:56_

---

_Closed by @charliermarsh on 2024-02-16 21:11_

---

_Referenced in [astral-sh/uv#1553](../../astral-sh/uv/issues/1553.md) on 2024-02-16 23:44_

---
