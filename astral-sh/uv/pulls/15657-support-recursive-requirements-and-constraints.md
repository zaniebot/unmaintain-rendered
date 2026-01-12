```yaml
number: 15657
title: Support recursive requirements and constraints inclusion
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/support-recursive-requirements-inclusion
created_at: 2025-09-03T12:08:59Z
updated_at: 2025-09-05T09:20:14Z
url: https://github.com/astral-sh/uv/pull/15657
synced_at: 2026-01-12T16:11:52Z
```

# Support recursive requirements and constraints inclusion

---

_@konstin_

uv currently panics with a stack overflow when requirements or constraints are recursively included. Instead, we ignore files we have already seen. The one complexity here is that we have to track whether we're in a requirements inclusion or in a constraints inclusion, to allow including a file separately for requirements and for constraints, and to handle `-r` inside or `-c` (which we treat as constraints too).

Fixes #15650

---

_Review requested from @charliermarsh by @konstin on 2025-09-03 12:08_

---

_Label `bug` added by @konstin on 2025-09-03 12:09_

---

_@konstin reviewed on 2025-09-03 12:09_

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:190 on 2025-09-03 12:09_

We have to use nested `&mut` to convince rustc that the inner `&mut FxHashSet` remain unique. 

---

_@charliermarsh reviewed on 2025-09-03 14:18_

---

_Review comment by @charliermarsh on `crates/uv-requirements-txt/src/lib.rs`:305 on 2025-09-03 14:18_

Consider using the return value of `requirements.insert` instead of the `contains`-`insert`?

---

_@charliermarsh approved on 2025-09-03 14:19_

---

_@charliermarsh reviewed on 2025-09-03 14:19_

---

_Review comment by @charliermarsh on `crates/uv-requirements-txt/src/lib.rs`:1389 on 2025-09-03 14:19_

Why do these need to be unowned? Why not `requirements: FxHashSet`?

---

_@charliermarsh reviewed on 2025-09-03 14:19_

---

_Review comment by @charliermarsh on `crates/uv-requirements-txt/src/lib.rs`:177 on 2025-09-03 14:19_

Consider a default impl?

---

_@konstin reviewed on 2025-09-03 15:06_

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:1389 on 2025-09-03 15:06_

I agree that they are ugly; What I'm trying to do is keep track of the visited files for requirements and constraints separately, but upon recursing into `-c`, keep track of only constraints, since `-r` nested in a `-c`'d file is also considered a constraint, like in the logic below.

```rust
                    // Switch to constraints mode, if we aren't in it already.
                    let mut visited = match visited {
                        VisitedFiles::Requirements { constraints, .. } => {
                            if !constraints.insert(sub_file.clone()) {
                                continue;
                            }
                            VisitedFiles::Constraints { constraints }
                        }
                        VisitedFiles::Constraints { constraints } => {
                            if !constraints.insert(sub_file.clone()) {
                                continue;
                            }
                            VisitedFiles::Constraints { constraints }
                        }
                    };
```

---

_@konstin reviewed on 2025-09-03 15:06_

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:177 on 2025-09-03 15:06_

It doesn't allow one for `&mut` unfortunately.

---

_@konstin reviewed on 2025-09-05 09:20_

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:1389 on 2025-09-05 09:20_

Going to merge this, I'll refactor it if someone knows how to write this in more elegant rust.

---

_Merged by @konstin on 2025-09-05 09:20_

---

_Closed by @konstin on 2025-09-05 09:20_

---

_Branch deleted on 2025-09-05 09:20_

---
