```yaml
number: 4065
title: Track supported Python range in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/requires-python
created_at: 2024-06-05T19:41:10Z
updated_at: 2024-06-06T07:52:33Z
url: https://github.com/astral-sh/uv/pull/4065
synced_at: 2026-01-10T13:54:02Z
```

# Track supported Python range in lockfile

---

_Pull request opened by @charliermarsh on 2024-06-05 19:41_

## Summary

This PR adds the `Requires-Python` range to the user's lockfile. This will enable us to validate it when installing.

For now, we repeat the `Requires-Python` back to the user; alternatively, though, we could detect the supported Python range automatically.

See: https://github.com/astral-sh/uv/issues/4052


---

_Label `preview` added by @charliermarsh on 2024-06-05 19:41_

---

_Marked ready for review by @charliermarsh on 2024-06-05 19:41_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-05 20:09_

---

_Review requested from @konstin by @charliermarsh on 2024-06-05 20:09_

---

_@zanieb reviewed on 2024-06-05 20:12_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolution/graph.rs`:278 on 2024-06-05 20:12_

Yeah this seems nice

---

_@zanieb approved on 2024-06-05 20:12_

---

_@charliermarsh reviewed on 2024-06-05 20:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:278 on 2024-06-05 20:21_

I think it's straightforward but need to try.

---

_Merged by @charliermarsh on 2024-06-05 20:22_

---

_Closed by @charliermarsh on 2024-06-05 20:22_

---

_Branch deleted on 2024-06-05 20:22_

---

_@konstin reviewed on 2024-06-06 07:52_

:100:

---
