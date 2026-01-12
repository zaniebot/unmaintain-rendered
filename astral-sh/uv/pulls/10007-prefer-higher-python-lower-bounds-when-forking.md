```yaml
number: 10007
title: Prefer higher Python lower-bounds when forking
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/min
created_at: 2024-12-18T16:31:17Z
updated_at: 2024-12-18T21:55:33Z
url: https://github.com/astral-sh/uv/pull/10007
synced_at: 2026-01-12T16:09:04Z
```

# Prefer higher Python lower-bounds when forking

---

_@charliermarsh_

## Summary

With the advent of `--fork-strategy requires-python` (the default), we actually _want_ to solve higher lower-bound forks before lower lower-bound forks. The former ensures we get the most compatible versions, while the latter ensures we get fewer overall versions. These two strategies match up with `--fork-strategy`, but need to be respected as such.

Closes https://github.com/astral-sh/uv/issues/9998.


---

_Label `bug` added by @charliermarsh on 2024-12-18 16:31_

---

_@charliermarsh reviewed on 2024-12-18 16:31_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:14198 on 2024-12-18 16:31_

Lots of splits, but this _is_ correct... A lot of packages dropped support for Python 3.7, but the user still wants to resolve for it.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-18 17:19_

---

_Review requested from @konstin by @charliermarsh on 2024-12-18 17:19_

---

_@BurntSushi approved on 2024-12-18 18:49_

Do the changes to the transformers resolution look right to you? I notice there are new dependencies being added.

Also, does this result in invalidating existing lock files if the order of the resolution markers would change? I imagine it would right?

---

_Merged by @charliermarsh on 2024-12-18 21:54_

---

_Closed by @charliermarsh on 2024-12-18 21:54_

---

_Branch deleted on 2024-12-18 21:54_

---

_Comment by @charliermarsh on 2024-12-18 21:55_

Yeah, they're right to me. Users can still opt-out. I don't believe existing lockfiles will be invalidated, since we don't look at the resolved markers when validating.

---
