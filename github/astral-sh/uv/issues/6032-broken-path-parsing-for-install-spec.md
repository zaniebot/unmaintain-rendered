---
number: 6032
title: Broken path parsing for install spec
type: issue
state: closed
author: flying-sheep
labels:
  - bug
assignees: []
created_at: 2024-08-12T11:27:36Z
updated_at: 2024-09-30T16:38:39Z
url: https://github.com/astral-sh/uv/issues/6032
synced_at: 2026-01-07T13:12:17-06:00
---

# Broken path parsing for install spec

---

_Issue opened by @flying-sheep on 2024-08-12 11:27_

```console
$ uv --version
uv 0.2.35
$ uv pip install --editable '/home/phil/Dev/Python/Packaging, Docs/session-info2'
error: Failed to parse: `/home/phil/Dev/Python/Packaging, Docs/session-info2`
  Caused by: Expected path (`/home/phil/Dev/Python/Packaging,`) to end in a supported file extension: `.whl`, `.zip`, `.tar.gz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
/home/phil/Dev/Python/Packaging, Docs/session-info2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Label `bug` added by @zanieb on 2024-08-12 12:11_

---

_Comment by @zanieb on 2024-08-12 12:13_

Thanks!

cc @charliermarsh might a regression from #5888 

---

_Comment by @charliermarsh on 2024-08-12 12:27_

I can take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-12 12:27_

---

_Comment by @flying-sheep on 2024-08-12 13:16_

Ah, sorry, I should have said that there is also an error with older versions before that PR (e.g. 0.2.32)

The underlying issue of something splitting the path at the space is older.

---

_Comment by @charliermarsh on 2024-08-12 14:37_

I think we currently require that you use a backslash here.

---

_Comment by @charliermarsh on 2024-08-12 14:37_

(But of course it should work as-is.)

---

_Comment by @flying-sheep on 2024-08-12 14:41_

I wasn’t aware that escape sequences in dependency specs are a thing at all!

---

_Comment by @charliermarsh on 2024-08-12 15:02_

Yeah it's a bit tricky. Like this doesn't work:

```
-e /Users/crmarsh/workspace/uv/foo bar/black_editable
```

But it does work if you quote it, or pass it directly on the command-line etc.


---

_Comment by @flying-sheep on 2024-08-12 15:34_

I am quoting it, see initial comment.

---

_Comment by @charliermarsh on 2024-08-12 15:35_

Yeah I'm describing what the behavior _should_ be, not what we do today.

---

_Comment by @flying-sheep on 2024-08-12 15:36_

I see! Thank you for clarifying!

---

_Referenced in [astral-sh/uv#7765](../../astral-sh/uv/issues/7765.md) on 2024-09-28 19:39_

---

_Referenced in [astral-sh/uv#7767](../../astral-sh/uv/pulls/7767.md) on 2024-09-28 21:23_

---

_Closed by @charliermarsh on 2024-09-30 16:38_

---

_Closed by @charliermarsh on 2024-09-30 16:38_

---
