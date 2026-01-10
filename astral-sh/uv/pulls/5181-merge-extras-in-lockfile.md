```yaml
number: 5181
title: Merge extras in lockfile
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/merge-extras-in-lockfile
created_at: 2024-07-18T10:48:18Z
updated_at: 2024-07-23T14:32:05Z
url: https://github.com/astral-sh/uv/pull/5181
synced_at: 2026-01-10T13:37:23Z
```

# Merge extras in lockfile

---

_Pull request opened by @konstin on 2024-07-18 10:48_

As user, you specify a list of extras. Internally, we decompose this into one virtual package per extra. We currently leak this abstraction by writing one entry per extra to the lockfile:

```toml
[[distribution]]
name = "foo"
version = "4.39.0.dev0"
source = { editable = "." }
dependencies = [
    { name = "pandas" },
    { name = "pandas", extra = "excel" },
    { name = "pandas", extra = "hdf5" },
    { name = "pandas", extra = "html", marker = "os_name != 'posix'" },
    { name = "pandas", extra = "output-formatting", marker = "os_name == 'posix'" },
    { name = "pandas", extra = "plot", marker = "os_name == 'posix'" },
]
```

Instead, we should merge the extras into a list of extras, creating a more concise lockfile:

```toml
[[distribution]]
name = "foo"
version = "4.39.0.dev0"
source = { editable = "." }
dependencies = [
    { name = "pandas", extra = ["excel", "hdf5"] },
    { name = "pandas", extra = ["html"], marker = "os_name != 'posix'" },
    { name = "pandas", extra = ["output-formatting", "plot"], marker = "os_name == 'posix'" },
]
```

The base package is now implicitly included, as it is in PEP 508.

Fixes #4888

---

_Label `enhancement` added by @konstin on 2024-07-18 10:48_

---

_Label `preview` added by @konstin on 2024-07-18 10:48_

---

_Review requested from @BurntSushi by @konstin on 2024-07-18 10:48_

---

_@charliermarsh reviewed on 2024-07-18 17:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:606 on 2024-07-18 17:50_

This makes `add_dependency` quadratic, right?

---

_@charliermarsh approved on 2024-07-18 17:51_

---

_@charliermarsh reviewed on 2024-07-18 18:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:606 on 2024-07-18 18:03_

I can't figure out a clean way around this.

---

_Merged by @charliermarsh on 2024-07-18 18:07_

---

_Closed by @charliermarsh on 2024-07-18 18:07_

---

_Branch deleted on 2024-07-18 18:07_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:606 on 2024-07-23 14:14_

This is only called from `Lock::from_resolution_graph` and it's un-exported, so it should be fine to build up some intermediate state that replaces this loop with a `O(1)` or `O(logn)` check.

---

_@BurntSushi reviewed on 2024-07-23 14:14_

---

_@charliermarsh reviewed on 2024-07-23 14:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:606 on 2024-07-23 14:19_

Totally agree. I briefly tried but had some trouble making it work.

---

_@konstin reviewed on 2024-07-23 14:32_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:606 on 2024-07-23 14:32_

For the large resolution transformers with all extras, this is the distribution of the number of iterations for the loop:

![image](https://github.com/user-attachments/assets/2a9d4bf3-2a68-44fd-bb7e-4c294165bac9)

We run the code inside the loop 2762 times.

---
