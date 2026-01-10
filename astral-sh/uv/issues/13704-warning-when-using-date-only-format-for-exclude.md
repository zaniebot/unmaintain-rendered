---
number: 13704
title: "Warning when using date-only format for `exclude-newer`"
type: issue
state: closed
author: johnthagen
labels:
  - documentation
assignees: []
created_at: 2025-05-28T19:34:18Z
updated_at: 2025-05-28T20:23:12Z
url: https://github.com/astral-sh/uv/issues/13704
synced_at: 2026-01-10T01:25:36Z
---

# Warning when using date-only format for `exclude-newer`

---

_Issue opened by @johnthagen on 2025-05-28 19:34_

### Summary

The [exclude-newer](https://docs.astral.sh/uv/reference/settings/#exclude-newer) docs say

> Accepts both [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339.html) timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system's configured time zone.

But using the more concise/simpler date format:

```toml
[tool.uv]
exclude-newer = "2025-04-01"
```

Results in a warning being printed on every usage:

```
$ uv sync
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 18, column 17
     |
  18 | exclude-newer = "2025-04-01"
     |                 ^^^^^^^^^^^^
  failed to find time component in "2025-04-01", which is required for parsing a timestamp

```

The short version is simpler, more concise, and much easier for a human to type manually. The docs say that it is supported, so I suggest that a warning should not be printed.

### Platform

macOS 15 arm64

### Version

uv 0.7.8 (0ddcc1905 2025-05-23)

### Python version

3.11.11

---

_Label `bug` added by @johnthagen on 2025-05-28 19:34_

---

_Renamed from "Warning when using date-only for `exclude-newer`" to "Warning when using date-only format for `exclude-newer`" by @johnthagen on 2025-05-28 19:34_

---

_Comment by @charliermarsh on 2025-05-28 19:35_

Thanks, that looks like a bug one way or another.

---

_Comment by @zanieb on 2025-05-28 19:40_

Hm, I'm confused. Prior discussion at

- https://github.com/astral-sh/uv/pull/8553

and subsequent documentation update at

- https://github.com/astral-sh/uv/pull/9135

It looks like we missed a spot there so the reference wasn't updated?

---

_Comment by @zanieb on 2025-05-28 19:44_

Ah it was only changed for https://docs.astral.sh/uv/reference/settings/#pip_exclude-newer

---

_Label `bug` removed by @zanieb on 2025-05-28 19:44_

---

_Label `documentation` added by @zanieb on 2025-05-28 19:44_

---

_Comment by @johnthagen on 2025-05-28 19:50_

@zanieb Would you accept a PR to fix the `exclude-newer` documentation and remove references to the shorter date-only format?

---

_Comment by @zanieb on 2025-05-28 19:52_

Yeah you're welcome to fix it, I think it's just a copy / paste from the pip reference? We _do_ support date-only on the CLI so we can't remove that from everywhere.

---

_Referenced in [astral-sh/uv#13706](../../astral-sh/uv/pulls/13706.md) on 2025-05-28 19:57_

---

_Comment by @johnthagen on 2025-05-28 19:57_

@zanieb 

- #13706

---

_Closed by @zanieb on 2025-05-28 20:23_

---
