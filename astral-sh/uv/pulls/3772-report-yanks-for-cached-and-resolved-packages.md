```yaml
number: 3772
title: Report yanks for cached and resolved packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
assignees: []
merged: true
base: main
head: charlie/y
created_at: 2024-05-22T20:34:15Z
updated_at: 2024-05-22T21:21:38Z
url: https://github.com/astral-sh/uv/pull/3772
synced_at: 2026-01-12T16:05:50Z
```

# Report yanks for cached and resolved packages

---

_@charliermarsh_

## Summary

We now show yanks as part of the resolution diagnostics, so they now appear for `sync`, `install`, `compile`, and any other operations. Further, they'll also appear for cached packages (but not packages that are _already_ installed).

Closes https://github.com/astral-sh/uv/issues/3768.

Closes #3766.


---

_Label `bug` added by @charliermarsh on 2024-05-22 20:34_

---

_Label `enhancement` added by @charliermarsh on 2024-05-22 20:34_

---

_@zanieb reviewed on 2024-05-22 20:52_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:4909 on 2024-05-22 20:52_

Whoops, didn't we intentionally display this last?

---

_@charliermarsh reviewed on 2024-05-22 20:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:4909 on 2024-05-22 20:54_

Is it significant?

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:4909 on 2024-05-22 20:57_

I think so, the installed list can be pretty long and it's not helpful if it's buried. We flagged this once already and moved it to the bottom.

---

_@zanieb reviewed on 2024-05-22 20:57_

---

_@zanieb approved on 2024-05-22 21:13_

---

_@charliermarsh reviewed on 2024-05-22 21:16_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:4909 on 2024-05-22 21:16_

So much harder to show them at the end lol

---

_@zanieb reviewed on 2024-05-22 21:18_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:4909 on 2024-05-22 21:18_

I know sorry 😬 

---

_Merged by @charliermarsh on 2024-05-22 21:21_

---

_Closed by @charliermarsh on 2024-05-22 21:21_

---

_Branch deleted on 2024-05-22 21:21_

---
