```yaml
number: 456
title: "Pin all resolver tests using `--exclude-newer`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/exclude-newer
created_at: 2023-11-19T15:00:02Z
updated_at: 2023-11-19T15:10:59Z
url: https://github.com/astral-sh/uv/pull/456
synced_at: 2026-01-12T16:03:57Z
```

# Pin all resolver tests using `--exclude-newer`

---

_@charliermarsh_

Uses yesterday's date, which should make it much less likely that our tests become stale over time.

Closes https://github.com/astral-sh/puffin/issues/449.

---

_Label `internal` added by @charliermarsh on 2023-11-19 15:00_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:53 on 2023-11-19 15:03_

Could we make this a constant, so it's easier to change if need be?

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:161 on 2023-11-19 15:03_

We really need more abstraction for those tests

---

_@konstin approved on 2023-11-19 15:05_

Did you check that all tests behave as expected? In that case r+

---

_Comment by @charliermarsh on 2023-11-19 15:06_

@konstin - Well there are no changes in the snapshots which seemed correct to me :)

And if I change to the same day but last year, many fail due to packages not existing (e.g., no Flask v3).

---

_Comment by @konstin on 2023-11-19 15:07_

> And if I change to the same day but last year, many fail due to packages not existing (e.g., no Flask v3).

That sounds good

---

_Merged by @charliermarsh on 2023-11-19 15:10_

---

_Closed by @charliermarsh on 2023-11-19 15:10_

---

_Branch deleted on 2023-11-19 15:10_

---
