```yaml
number: 12726
title: "Avoid infinite loop in `uv export` with conflicts"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2025-04-07T18:49:14Z
updated_at: 2025-04-07T19:10:59Z
url: https://github.com/astral-sh/uv/pull/12726
synced_at: 2026-01-12T16:10:22Z
```

# Avoid infinite loop in `uv export` with conflicts

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/12695.

Closes https://github.com/astral-sh/uv/issues/12719.


---

_@charliermarsh reviewed on 2025-04-07 18:49_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:500 on 2025-04-07 18:49_

I think this was just an oversight. This push is in the `match` above, with proper gating.

---

_@zanieb approved on 2025-04-07 18:51_

Can you add the changelog entry too?

---

_Label `bug` added by @zanieb on 2025-04-07 18:52_

---

_Merged by @charliermarsh on 2025-04-07 19:10_

---

_Closed by @charliermarsh on 2025-04-07 19:10_

---

_Branch deleted on 2025-04-07 19:10_

---
