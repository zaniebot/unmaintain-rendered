```yaml
number: 2194
title: "Error when editables don't match `Requires-Python`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-03-05T04:00:30Z
updated_at: 2024-03-05T04:20:50Z
url: https://github.com/astral-sh/uv/pull/2194
synced_at: 2026-01-10T14:54:43Z
```

# Error when editables don't match `Requires-Python`

---

_Pull request opened by @charliermarsh on 2024-03-05 04:00_

## Summary

Closes https://github.com/astral-sh/uv/issues/2192.

## Test Plan

`cargo test`


---

_Review comment by @danielhollas on `crates/uv/src/commands/pip_install.rs`:395 on 2024-03-05 04:05_

Not that I know anything about `uv` codebase but FTR this fix should also be applied on non-editable local installs. (appologies if that's already done).

Thanks for working on this!

---

_@danielhollas reviewed on 2024-03-05 04:05_

---

_Marked ready for review by @charliermarsh on 2024-03-05 04:11_

---

_@charliermarsh reviewed on 2024-03-05 04:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:395 on 2024-03-05 04:14_

I thought we already enforced those, but I guess not! Will do it in a separate PR, thanks.

---

_Merged by @charliermarsh on 2024-03-05 04:20_

---

_Closed by @charliermarsh on 2024-03-05 04:20_

---

_Branch deleted on 2024-03-05 04:20_

---
