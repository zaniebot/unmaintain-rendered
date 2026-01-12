```yaml
number: 8477
title: "Add `--no-group` support to CLI"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: tracking/735
head: charlie/no-group
created_at: 2024-10-22T19:31:03Z
updated_at: 2024-10-22T20:18:41Z
url: https://github.com/astral-sh/uv/pull/8477
synced_at: 2026-01-12T16:08:20Z
```

# Add `--no-group` support to CLI

---

_@charliermarsh_

## Summary

Now that `default-groups` can include more than just `"dev"`, it makes sense to allow users to remove groups with `--no-group`.


---

_Label `enhancement` added by @charliermarsh on 2024-10-22 19:31_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 19:33_

---

_@zanieb reviewed on 2024-10-22 19:35_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2623 on 2024-10-22 19:35_

I don't think this conflicts? What if I do `--only-group foo --only-group bar --no-group foo`? Per previous discussions about overriding prior options, this seems nice to permit.

---

_@zanieb reviewed on 2024-10-22 19:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1516 on 2024-10-22 19:36_

What about a test for `--group foo --no-group foo`?

(not necessarily in this function)

---

_@zanieb approved on 2024-10-22 19:36_

---

_@charliermarsh reviewed on 2024-10-22 20:12_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2623 on 2024-10-22 20:12_

Ok, I went back and forth on this.

---

_@zanieb reviewed on 2024-10-22 20:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2623 on 2024-10-22 20:13_

I trust your judgement, I was just surprised.

---

_Merged by @charliermarsh on 2024-10-22 20:18_

---

_Closed by @charliermarsh on 2024-10-22 20:18_

---

_Branch deleted on 2024-10-22 20:18_

---

_@charliermarsh reviewed on 2024-10-22 20:18_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2623 on 2024-10-22 20:18_

It just added a bit of complexity to the implementation, so I had punted it. Makes sense though.

---
