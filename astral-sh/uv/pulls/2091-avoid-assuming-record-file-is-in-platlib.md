```yaml
number: 2091
title: "Avoid assuming `RECORD` file is in `platlib`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rec
created_at: 2024-02-29T17:17:10Z
updated_at: 2024-02-29T17:21:50Z
url: https://github.com/astral-sh/uv/pull/2091
synced_at: 2026-01-12T16:04:51Z
```

# Avoid assuming `RECORD` file is in `platlib`

---

_@charliermarsh_

## Summary

This was a missed find-and-replace. We shouldn't assume `layout.platlib` here, since `RECORD` will be written to `site_packages` (which could be `layout.purelib`).

This is hard to reproduce. You need a _fresh_ environment where `purelib` and `platlib` differ (which isn't the case for virtualenvs, at least typically), and you need to be installing a new package that is a purelib. I tested it by manually changing `platlib` to point to a different path.

Closes https://github.com/astral-sh/uv/issues/2064.


---

_Label `bug` added by @charliermarsh on 2024-02-29 17:17_

---

_Marked ready for review by @charliermarsh on 2024-02-29 17:17_

---

_Merged by @charliermarsh on 2024-02-29 17:21_

---

_Closed by @charliermarsh on 2024-02-29 17:21_

---

_Branch deleted on 2024-02-29 17:21_

---
