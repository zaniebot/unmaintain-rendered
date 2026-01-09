---
number: 13114
title: "Support GraalPy install with `uv python`"
type: issue
state: open
author: paugier
labels:
  - enhancement
assignees: []
created_at: 2025-04-26T03:38:20Z
updated_at: 2025-04-28T18:23:56Z
url: https://github.com/astral-sh/uv/issues/13114
synced_at: 2026-01-07T13:12:18-06:00
---

# Support GraalPy install with `uv python`

---

_Issue opened by @paugier on 2025-04-26 03:38_

There is already some kind of support for GraalPy (see https://github.com/astral-sh/uv/issues/4197 and https://github.com/astral-sh/uv/pull/5141).

However, it seems that one cannot download/install GraalPy with UV:

```
$ uv python install graalpy
error: No download found for request: graalpy-any-linux-x86_64-gnu
```

It would be very convenient to be able to install GraalPy with UV since the current alternatives (see https://github.com/oracle/graalpython?tab=readme-ov-file#getting-started) are pyenv (which is quite inconvenient and I think could now be avoided for people that don't need to compile CPython) and manual install (which is a bit boring).

GraalPy binaries are available on Github (https://github.com/oracle/graalpython/releases) so this should not be difficult for UV to install them.

@timfel, you might be interested in this issue since you worked on #5141.


---

_Label `enhancement` added by @paugier on 2025-04-26 03:38_

---

_Referenced in [astral-sh/uv#13172](../../astral-sh/uv/pulls/13172.md) on 2025-04-28 09:26_

---

_Comment by @timfel on 2025-04-28 18:23_

@paugier https://github.com/astral-sh/uv/pull/13172

---
