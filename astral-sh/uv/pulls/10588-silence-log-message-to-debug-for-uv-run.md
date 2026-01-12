```yaml
number: 10588
title: Silence log message to debug for uv run
type: pull_request
state: merged
author: rumpelsepp
labels:
  - cli
assignees: []
merged: true
base: main
head: run-logging
created_at: 2025-01-14T09:20:59Z
updated_at: 2025-01-15T19:05:03Z
url: https://github.com/astral-sh/uv/pull/10588
synced_at: 2026-01-12T16:09:23Z
```

# Silence log message to debug for uv run

---

_@rumpelsepp_

## Summary

This log message is shown every time a script including a uv
shebang is run. After installing all dependencies, printing this log
message every time does not add any relevant information for the user. I
would say it could even be misleading and motivate the user to debug his
own program searching for this log message.

As a consequence, reduce the log level of this message to debug.

## Test Plan

uv run was called with default settings and the log message didn't show up.
cargo test was run and I tried to fix the issues.


---

_Label `needs-decision` added by @charliermarsh on 2025-01-14 18:27_

---

_Label `cli` added by @charliermarsh on 2025-01-14 18:27_

---

_@zanieb approved on 2025-01-14 20:47_

I think I'm in favor.

---

_Merged by @zanieb on 2025-01-15 19:04_

---

_Closed by @zanieb on 2025-01-15 19:04_

---

_Label `needs-decision` removed by @zanieb on 2025-01-15 19:04_

---

_Comment by @zanieb on 2025-01-15 19:05_

Thanks for contributing!

---
