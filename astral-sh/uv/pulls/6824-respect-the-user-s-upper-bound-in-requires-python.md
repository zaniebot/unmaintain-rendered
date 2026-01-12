```yaml
number: 6824
title: "Respect the user's upper-bound in `requires-python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/upper
created_at: 2024-08-29T16:59:13Z
updated_at: 2024-08-29T18:37:07Z
url: https://github.com/astral-sh/uv/pull/6824
synced_at: 2026-01-12T16:07:32Z
```

# Respect the user's upper-bound in `requires-python`

---

_@charliermarsh_

## Summary

We now respect the user-provided upper-bound in for `requires-python`. So, if the user has `requires-python = "==3.11.*"`, we won't explore forks that have `python_version >= '3.12'`, for example.

However, we continue to _only_ compare the lower bounds when assessing whether a dependency is compatible with a given Python range.

Closes https://github.com/astral-sh/uv/issues/6150.


---

_Label `enhancement` added by @charliermarsh on 2024-08-29 16:59_

---

_Marked ready for review by @charliermarsh on 2024-08-29 18:05_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-29 18:05_

---

_Review requested from @konstin by @charliermarsh on 2024-08-29 18:05_

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:337 on 2024-08-29 18:16_

This should definitely be part of pubgrub, can you open an issue?

---

_Review comment by @konstin on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:7 on 2024-08-29 18:16_

:tada: 

---

_@konstin approved on 2024-08-29 18:17_

We should check that this doesn't collide with #6268, otherwise, important improvement!

---

_@BurntSushi approved on 2024-08-29 18:19_

LGTM!

---

_Comment by @BurntSushi on 2024-08-29 18:20_

> We should check that this doesn't collide with #6268, otherwise, important improvement!

Yeah I was just talking to Charlie about this. I don't _think_ it does.

---

_@charliermarsh reviewed on 2024-08-29 18:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:337 on 2024-08-29 18:30_

https://github.com/pubgrub-rs/pubgrub/issues/253

---

_Merged by @charliermarsh on 2024-08-29 18:37_

---

_Closed by @charliermarsh on 2024-08-29 18:37_

---

_Branch deleted on 2024-08-29 18:37_

---
