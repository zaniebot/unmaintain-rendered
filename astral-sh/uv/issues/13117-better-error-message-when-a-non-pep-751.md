```yaml
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
synced_at: 2026-01-12T16:01:20Z
```

# Better error message when a non-PEP-751-compatible filename lock file is used

---

_@ReinforcedKnowledge_

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

_Assigned to @charliermarsh by @charliermarsh on 2025-04-26 14:48_

---

_Closed by @zanieb on 2025-04-29 21:37_

---
