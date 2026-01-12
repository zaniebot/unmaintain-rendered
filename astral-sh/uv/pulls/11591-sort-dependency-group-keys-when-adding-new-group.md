```yaml
number: 11591
title: Sort dependency group keys when adding new group
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/sorted-entries
created_at: 2025-02-18T09:25:03Z
updated_at: 2025-02-28T09:33:27Z
url: https://github.com/astral-sh/uv/pull/11591
synced_at: 2026-01-12T16:09:54Z
```

# Sort dependency group keys when adding new group

---

_@jtfmumm_

This change keeps dependency group keys sorted when adding new ones. 

If earlier dependency group keys were not sorted, we just append the new group key to avoid churn in `pyproject.toml`. See discussion on #11447. I've added a new snapshot test to capture this case.

Closes #11447. 


---

_Review requested from @zanieb by @jtfmumm on 2025-02-18 10:16_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:4829 on 2025-02-18 14:25_

Can we add a few test cases where there are comments between the groups, to understand how they move around?

---

_@charliermarsh reviewed on 2025-02-18 14:25_

---

_@zanieb reviewed on 2025-02-18 14:32_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:4829 on 2025-02-18 14:32_

👍 this is an unfortunately frequent cause of bug reports

---

_@zanieb reviewed on 2025-02-18 14:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:4830 on 2025-02-18 14:33_

Random, but it doesn't actually look like you need the filters for this snapshot.  You could reduce nesting by omitting them — but I don't mind either way.

---

_@jtfmumm reviewed on 2025-02-18 15:56_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:4829 on 2025-02-18 15:56_

Added two new tests

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:4830 on 2025-02-18 15:56_

Removed filters on these tests

---

_@jtfmumm reviewed on 2025-02-18 15:56_

---

_@T-256 reviewed on 2025-02-18 16:43_

---

_Review comment by @T-256 on `crates/uv/tests/it/edit.rs`:4932 on 2025-02-18 16:43_

Could sort `default-groups` separately and put them in top-most priority?

(it is implicitly set to `tool.uv.default-groups = ["dev"]`)

---

_@zanieb reviewed on 2025-02-18 17:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:4932 on 2025-02-18 17:45_

I think I'd prefer not to do that, otherwise when you change your default groups the ordering is unsorted and we won't respect it anymore. Though it is interesting.

---

_@zanieb approved on 2025-02-18 17:45_

---

_@charliermarsh approved on 2025-02-18 17:46_

---

_@charliermarsh reviewed on 2025-02-18 17:48_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject_mut.rs`:458 on 2025-02-18 17:48_

The logic we use for detecting sorted dependencies is a bit more complex. Specifically, we check whether the values are sorted case-insensitively or case-sensitively, and then respect that sort if so. We _could_ do the same here, but I don't consider it blocking (it seems pretty rare for users to use capitalization in these identifiers, unlike in package names).

---

_Merged by @jtfmumm on 2025-02-18 18:12_

---

_Closed by @jtfmumm on 2025-02-18 18:12_

---

_Branch deleted on 2025-02-18 18:12_

---

_@T-256 reviewed on 2025-02-18 20:01_

---

_Review comment by @T-256 on `crates/uv/tests/it/edit.rs`:4932 on 2025-02-18 20:01_

Correct, I would like to see it as a lint rule in future.

---

_Label `enhancement` added by @jtfmumm on 2025-02-28 09:29_

---

_Label `enhancement` removed by @jtfmumm on 2025-02-28 09:33_

---
