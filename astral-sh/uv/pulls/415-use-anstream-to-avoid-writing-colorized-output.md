```yaml
number: 415
title: "Use `anstream` to avoid writing colorized output"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/colors
created_at: 2023-11-13T19:50:15Z
updated_at: 2023-11-13T20:00:14Z
url: https://github.com/astral-sh/uv/pull/415
synced_at: 2026-01-10T15:50:28Z
```

# Use `anstream` to avoid writing colorized output

---

_Pull request opened by @charliermarsh on 2023-11-13 19:50_

A more robust solution to avoiding colorized output by ensuring we write to `stdout` and `stderr` via the [`anstream`](https://docs.rs/anstream/latest/anstream/) crate.

Closes https://github.com/astral-sh/puffin/issues/393.


---

_Marked ready for review by @charliermarsh on 2023-11-13 19:55_

---

_Label `internal` added by @charliermarsh on 2023-11-13 19:55_

---

_Merged by @charliermarsh on 2023-11-13 20:00_

---

_Closed by @charliermarsh on 2023-11-13 20:00_

---

_Branch deleted on 2023-11-13 20:00_

---
