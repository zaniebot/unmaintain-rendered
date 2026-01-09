---
number: 13117
title: Better error message when a non-PEP-751-compatible filename lock file is used
type: issue
state: closed
author: ReinforcedKnowledge
labels:
  - error messages
assignees: []
created_at: 2025-04-26T04:09:59Z
updated_at: 2025-04-29T21:37:07Z
url: https://github.com/astral-sh/uv/issues/13117
synced_at: 2026-01-07T13:12:18-06:00
---

# Better error message when a non-PEP-751-compatible filename lock file is used

---

_Issue opened by @ReinforcedKnowledge on 2025-04-26 04:09_

### Summary

Hi!

I think it'd be great if we changed the error produced by the commands that accept PEP-751 compatible lock files when the file name doesn't follow PEP-751 recommendations (either `pylock.toml` or `r"^pylock\.([^.]+)\.toml$"`).

Currently, we get an error to parse requirement in {filename}. Maybe there are better ways to word this :)

Steps to reproduce:
```
uv init test
uv add pandas  # or any other cached package you want
uv export --format pylock.toml > pylock.toml
uv export --format pylock.toml > test.toml
```

Doing `uv pip sync test.toml` errors:
```
error: Couldn't parse requirement in `test.toml` at position 99
  Caused by: no such comparison operator "=", must be one of ~= == != <= >= < > ===
lock-version = "1.0"
             ^^^^^^^
```

While `uv pip sync pylock.toml` works.

### Example

_No response_

---

_Label `enhancement` added by @ReinforcedKnowledge on 2025-04-26 04:09_

---

_Label `enhancement` removed by @charliermarsh on 2025-04-26 14:06_

---

_Label `error messages` added by @charliermarsh on 2025-04-26 14:06_

---

_Comment by @charliermarsh on 2025-04-26 14:10_

Good call, I will fix.

---

_Referenced in [astral-sh/uv#13119](../../astral-sh/uv/pulls/13119.md) on 2025-04-26 14:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-26 14:48_

---

_Referenced in [astral-sh/uv#13126](../../astral-sh/uv/issues/13126.md) on 2025-04-27 07:48_

---

_Closed by @zanieb on 2025-04-29 21:37_

---
