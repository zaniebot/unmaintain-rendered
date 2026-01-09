---
number: 4603
title: "No error when PEP508 source and `tool.uv.sources` entry don't match"
type: issue
state: closed
author: ibraheemdev
labels:
  - preview
assignees: []
created_at: 2024-06-27T22:05:52Z
updated_at: 2024-08-12T23:46:45Z
url: https://github.com/astral-sh/uv/issues/4603
synced_at: 2026-01-07T13:12:17-06:00
---

# No error when PEP508 source and `tool.uv.sources` entry don't match

---

_Issue opened by @ibraheemdev on 2024-06-27 22:05_

For example:
```
dependencies = [
    "uv-public-pypackage @ https://github.com/astral-test/uv-public-pypackage@0.0.2",
]

[tool.uv.sources]
uv-public-pypackage = { git = "https://github.com/astral-test/uv-public-pypackage", tag = "0.0.1" }
```

`uv lock/sync` currently run without error, preferring `tool.uv.sources`.

---

_Label `preview` added by @ibraheemdev on 2024-06-27 22:05_

---

_Referenced in [astral-sh/uv#4604](../../astral-sh/uv/issues/4604.md) on 2024-06-27 22:08_

---

_Renamed from "Error when PEP508 source and `tool.uv.sources` entry don't match" to "No error when PEP508 source and `tool.uv.sources` entry don't match" by @ibraheemdev on 2024-06-27 22:09_

---

_Referenced in [astral-sh/uv#3943](../../astral-sh/uv/issues/3943.md) on 2024-07-09 10:05_

---

_Referenced in [astral-sh/uv#5341](../../astral-sh/uv/issues/5341.md) on 2024-07-23 16:04_

---

_Referenced in [astral-sh/uv#5383](../../astral-sh/uv/issues/5383.md) on 2024-07-24 14:38_

---

_Comment by @charliermarsh on 2024-08-12 23:46_

I added test coverage to at least codify the existing behavior in https://github.com/astral-sh/uv/pull/6051.

---

_Comment by @charliermarsh on 2024-08-12 23:46_

I found that we actually error here? So gonna close.

---

_Closed by @charliermarsh on 2024-08-12 23:46_

---
