---
number: 11804
title: "uv doesn't complain if two indexes defined in `pyproject.toml` have the same name"
type: issue
state: closed
author: jtfmumm
labels:
  - good first issue
  - help wanted
  - error messages
assignees: []
created_at: 2025-02-26T17:10:11Z
updated_at: 2025-02-27T21:33:59Z
url: https://github.com/astral-sh/uv/issues/11804
synced_at: 2026-01-07T13:12:18-06:00
---

# uv doesn't complain if two indexes defined in `pyproject.toml` have the same name

---

_Issue opened by @jtfmumm on 2025-02-26 17:10_

### Summary

If I create a new project with `uv init` and  add two index specifications with the same name to `pyproject.toml`, I can run `uv add` without encountering an error.

```
[[tool.uv.index]]
name = "alpha_b"
url = "<omitted>"

[[tool.uv.index]]
name = "alpha_b"
url = "<omitted>"
```

I would expect that this would result in at least a warning. For example, it could lead to surprising behavior when interacting with the in-progress [index proxy url work][https://github.com/astral-sh/uv/pull/11782].

### Platform

macOS 13.5 

### Version

0.5.21

### Python version

_No response_

---

_Label `bug` added by @jtfmumm on 2025-02-26 17:10_

---

_Comment by @charliermarsh on 2025-02-26 17:53_

This is arguably intentional because you can have indexes with the same name defined across files (and we'd then combine the lists of indexes), and we have well-defined priority for how they're respected.

---

_Comment by @zanieb on 2025-02-26 18:10_

That makes sense to me — and I think overrides are important in the context of `--index <name>=<url>` too.

I think we could warn or error if they're repeated in a single file though.

---

_Comment by @charliermarsh on 2025-02-26 18:11_

Error in the same file would make sense, I think.

---

_Label `good first issue` added by @charliermarsh on 2025-02-27 00:06_

---

_Label `help wanted` added by @charliermarsh on 2025-02-27 00:06_

---

_Label `bug` removed by @charliermarsh on 2025-02-27 00:06_

---

_Label `error messages` added by @charliermarsh on 2025-02-27 00:06_

---

_Referenced in [astral-sh/uv#11824](../../astral-sh/uv/pulls/11824.md) on 2025-02-27 07:09_

---

_Closed by @charliermarsh on 2025-02-27 21:33_

---
