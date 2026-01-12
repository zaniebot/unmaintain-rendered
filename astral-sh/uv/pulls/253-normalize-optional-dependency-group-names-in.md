```yaml
number: 253
title: Normalize optional dependency group names in pyproject files
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/extra-norm
created_at: 2023-10-31T15:46:46Z
updated_at: 2023-10-31T19:15:01Z
url: https://github.com/astral-sh/uv/pull/253
synced_at: 2026-01-12T16:03:49Z
```

# Normalize optional dependency group names in pyproject files

---

_@zanieb_

Going to add some tests.

Extends #239 
Closes #245 

Normalizes optional dependency group names found in pyproject files before comparing them to the normalized user-requested extras.

---

_Converted to draft by @zanieb on 2023-10-31 15:47_

---

_Renamed from "Normalize extra names in pyproject files" to "Normalize optional dependency group names in pyproject files" by @zanieb on 2023-10-31 15:51_

---

_Comment by @zanieb on 2023-10-31 15:53_

Alternatively, we could derive a type from the `PyProjectToml` type e.g.`PyProject` or `NormalizedPyProjectToml` which has normalized types in the struct itself — this seems generally useful.

---

_Marked ready for review by @zanieb on 2023-10-31 17:01_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:94 on 2023-10-31 17:22_

Is this necessary? I feel like it shouldn't be necessary to return a vector here rather than an iterator. (Though I think this code disappears in your other branch, maybe...)

---

_@charliermarsh approved on 2023-10-31 17:23_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:94 on 2023-10-31 18:56_

```
error[E0515]: cannot return value referencing function parameter `optional_dependencies`
  --> crates/puffin-cli/src/requirements.rs:85:37
   |
85 |   ...                   optional_dependencies
   |                         ^--------------------
   |                         |
   |  _______________________`optional_dependencies` is borrowed here
   | |
86 | | ...                       .iter()
87 | | ...                       .flat_map(|(name, requirements)| {
88 | | ...                           if extras.contains(&ExtraName::normalize(name)) {
...  |
92 | | ...                           }
93 | | ...                       })
   | |____________________________^ returns a value referencing data owned by the current function
   |
   = help: use `.collect()` to allocate the iterator
```

---

_@zanieb reviewed on 2023-10-31 18:57_

---

_@zanieb reviewed on 2023-10-31 18:57_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:94 on 2023-10-31 18:57_

It does disappear though

---

_Merged by @zanieb on 2023-10-31 19:15_

---

_Closed by @zanieb on 2023-10-31 19:15_

---

_Branch deleted on 2023-10-31 19:15_

---
