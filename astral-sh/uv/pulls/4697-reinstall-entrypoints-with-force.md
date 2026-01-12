```yaml
number: 4697
title: "Reinstall entrypoints with `--force`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2024-07-01T14:05:14Z
updated_at: 2024-07-01T14:52:48Z
url: https://github.com/astral-sh/uv/pull/4697
synced_at: 2026-01-12T16:06:24Z
```

# Reinstall entrypoints with `--force`

---

_@charliermarsh_

## Summary

I think this may have just been a typo.

Closes https://github.com/astral-sh/uv/issues/4692.

## Test Plan

Run `cargo run tool install flask --force --reinstall` repeatedly.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-01 14:05_

---

_Label `bug` added by @charliermarsh on 2024-07-01 14:05_

---

_Label `preview` added by @charliermarsh on 2024-07-01 14:05_

---

_Comment by @zanieb on 2024-07-01 14:12_

Huh? Isn't there test coverage for this?

---

_@zanieb approved on 2024-07-01 14:12_

---

_Comment by @charliermarsh on 2024-07-01 14:15_

I think only for the case in which the existing entrypoint was _not_ installed by `uv`. I will amend.

---

_Merged by @charliermarsh on 2024-07-01 14:25_

---

_Closed by @charliermarsh on 2024-07-01 14:25_

---

_Branch deleted on 2024-07-01 14:25_

---

_Comment by @zanieb on 2024-07-01 14:52_

Thanks!

---
