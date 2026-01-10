```yaml
number: 15087
title: "Support `match-runtime = true` in the `uv pip` CLI"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/match-runtime-pip
created_at: 2025-08-05T17:53:03Z
updated_at: 2025-08-05T20:03:11Z
url: https://github.com/astral-sh/uv/pull/15087
synced_at: 2026-01-10T06:44:33Z
```

# Support `match-runtime = true` in the `uv pip` CLI

---

_Pull request opened by @charliermarsh on 2025-08-05 17:53_

## Summary

Pretty straightforward, a ~one line change plus recreating the `BuildDispatch` (which I tried to avoid, but ran into lifetime issues).


---

_@charliermarsh reviewed on 2025-08-05 17:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/install.rs`:566 on 2025-08-05 17:56_

This is `build_dispatch.with_extra_build_requires(&extra_build_requires)` but the lifetimes get rejected. I'll try again.

---

_Review requested from @zanieb by @charliermarsh on 2025-08-05 17:58_

---

_Review requested from @konstin by @charliermarsh on 2025-08-05 17:58_

---

_Label `enhancement` added by @charliermarsh on 2025-08-05 17:58_

---

_Label `preview` added by @charliermarsh on 2025-08-05 17:58_

---

_Marked ready for review by @charliermarsh on 2025-08-05 17:58_

---

_@zanieb approved on 2025-08-05 18:14_

---

_Merged by @charliermarsh on 2025-08-05 20:03_

---

_Closed by @charliermarsh on 2025-08-05 20:03_

---

_Branch deleted on 2025-08-05 20:03_

---
