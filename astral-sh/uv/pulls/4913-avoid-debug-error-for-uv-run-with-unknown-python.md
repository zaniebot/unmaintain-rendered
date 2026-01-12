```yaml
number: 4913
title: "Avoid debug error for `uv run` with unknown Python version"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: charlie/err-msg
created_at: 2024-07-09T05:22:26Z
updated_at: 2024-07-09T05:29:56Z
url: https://github.com/astral-sh/uv/pull/4913
synced_at: 2026-01-12T16:06:31Z
```

# Avoid debug error for `uv run` with unknown Python version

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/4848.

## Test Plan

```
> cargo run -- run -vv --preview --isolated --python 3.12.4 python -V
error: No interpreter found for Python 3.12.4 in virtual environments or managed installations or system path
```

---

_Label `error messages` added by @charliermarsh on 2024-07-09 05:22_

---

_Label `preview` added by @charliermarsh on 2024-07-09 05:22_

---

_Merged by @charliermarsh on 2024-07-09 05:29_

---

_Closed by @charliermarsh on 2024-07-09 05:29_

---

_Branch deleted on 2024-07-09 05:29_

---
