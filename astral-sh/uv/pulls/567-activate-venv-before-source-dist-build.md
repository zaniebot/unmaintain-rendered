```yaml
number: 567
title: Activate venv before source dist build
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/activate-venv-before-build
created_at: 2023-12-05T12:58:07Z
updated_at: 2023-12-12T14:46:39Z
url: https://github.com/astral-sh/uv/pull/567
synced_at: 2026-01-12T16:04:02Z
```

# Activate venv before source dist build

---

_@konstin_

Fixes #552

---

_Comment by @konstin on 2023-12-05 12:58_

Current dependencies on/for this PR:
* main
  * **PR #564** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/564?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #565** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #566** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #567** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
          * **PR #587** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2023-12-12 14:34_

---

_@charliermarsh approved on 2023-12-12 14:43_

---

_@charliermarsh reviewed on 2023-12-12 14:44_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:699 on 2023-12-12 14:44_

I sometimes like to scope the mutability, like...

```rust
let path = {
    let mut path = venv.bin_dir().into_os_string();
    if let Some(existing) = env::var_os("PATH") {
        ...
    }
    path
};
```

---

_@charliermarsh reviewed on 2023-12-12 14:44_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:699 on 2023-12-12 14:44_

Totally optional comment

---

_Merged by @konstin on 2023-12-12 14:46_

---

_Closed by @konstin on 2023-12-12 14:46_

---

_Branch deleted on 2023-12-12 14:46_

---
