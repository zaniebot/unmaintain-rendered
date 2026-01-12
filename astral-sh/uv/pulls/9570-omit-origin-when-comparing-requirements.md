```yaml
number: 9570
title: Omit origin when comparing requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/origin
created_at: 2024-12-02T02:41:58Z
updated_at: 2024-12-03T15:52:20Z
url: https://github.com/astral-sh/uv/pull/9570
synced_at: 2026-01-12T16:08:52Z
```

# Omit origin when comparing requirements

---

_@charliermarsh_

## Summary

I noticed that we consider two requirements to be different if they come from different files. This seems like an oversight.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-02 02:42_

---

_Label `bug` added by @charliermarsh on 2024-12-02 02:42_

---

_@charliermarsh reviewed on 2024-12-02 02:42_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/tool_install.rs`:1977 on 2024-12-02 02:42_

As an example: we didn't detect that this install request was identical to the previous, because in this latter case, the requirement came from a `requirements.txt` file rather than the CLI.

---

_@zanieb approved on 2024-12-02 23:21_

---

_Merged by @charliermarsh on 2024-12-02 23:22_

---

_Closed by @charliermarsh on 2024-12-02 23:22_

---

_Branch deleted on 2024-12-02 23:22_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/requirement.rs`:135 on 2024-12-03 13:06_

I think you need custom `Ord`/`PartialOrd` trait impls too, right? Otherwise, those will incorporate `origin` for comparisons and I think result in an inconsistency with your `Eq`/`PartialEq` trait impls.

---

_@BurntSushi reviewed on 2024-12-03 13:07_

---

_@zanieb reviewed on 2024-12-03 14:49_

---

_Review comment by @zanieb on `crates/uv-pypi-types/src/requirement.rs`:135 on 2024-12-03 14:49_

Classic :D

---

_@charliermarsh reviewed on 2024-12-03 15:52_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/requirement.rs`:135 on 2024-12-03 15:52_

Yeah this is truly classic. Thank you.

---
