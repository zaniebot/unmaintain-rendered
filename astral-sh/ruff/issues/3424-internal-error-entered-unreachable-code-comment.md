```yaml
number: 3424
title: "'internal error: entered unreachable code: Comment should contain '#' character'"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-09T20:59:06Z
updated_at: 2023-03-13T04:02:51Z
url: https://github.com/astral-sh/ruff/issues/3424
synced_at: 2026-01-10T11:09:46Z
```

# 'internal error: entered unreachable code: Comment should contain '#' character'

---

_Issue opened by @qarmin on 2023-03-09 20:59_

0.0.254

```
thread 'main' panicked at 'internal error: entered unreachable code: Comment should contain '#' character', crates/ruff/src/rules/eradicate/rules.rs:45:5
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```
[v1_resource_quota_list31.py.zip](https://github.com/charliermarsh/ruff/files/10936011/v1_resource_quota_list31.py.zip)


---

_Comment by @charliermarsh on 2023-03-09 21:13_

I think that contains a CR line ending which is probably the issue.

---

_Label `bug` added by @charliermarsh on 2023-03-10 05:30_

---

_Comment by @charliermarsh on 2023-03-13 04:02_

Resolved via #3454 and #3439.

---

_Closed by @charliermarsh on 2023-03-13 04:02_

---
