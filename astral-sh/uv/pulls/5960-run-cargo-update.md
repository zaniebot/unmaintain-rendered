```yaml
number: 5960
title: "Run `cargo update`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/update
created_at: 2024-08-09T15:19:56Z
updated_at: 2024-08-09T17:36:17Z
url: https://github.com/astral-sh/uv/pull/5960
synced_at: 2026-01-12T16:07:07Z
```

# Run `cargo update`

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-08-09 15:19_

---

_Comment by @charliermarsh on 2024-08-09 15:20_

We now have three versions of `windows-sys`, maybe we can dedupe that.

---

_Comment by @konstin on 2024-08-09 15:43_

https://github.com/colored-rs/colored seems to be a main culprit for old windows-sys

---

_Merged by @charliermarsh on 2024-08-09 17:36_

---

_Closed by @charliermarsh on 2024-08-09 17:36_

---

_Branch deleted on 2024-08-09 17:36_

---

_Comment by @charliermarsh on 2024-08-09 17:36_

I think that's only used in bench.

---
