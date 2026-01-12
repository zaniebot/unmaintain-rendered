```yaml
number: 10490
title: Avoid forking for identical markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2025-01-11T01:59:34Z
updated_at: 2025-01-11T13:06:22Z
url: https://github.com/astral-sh/uv/pull/10490
synced_at: 2026-01-12T16:09:19Z
```

# Avoid forking for identical markers

---

_@charliermarsh_

## Summary

If you have a dependency with a marker, and you add a constraint, it causes us to _always_ fork, because we represent the constraint as a second dependency with the marker repeated (and, therefore, we have two requirements of the same name, both with markers). I don't think we should fork here -- and in the end it's leading to this undesirable resolution: #10481.

I tried to change constraints such that we just _reuse_ and augment the initial requirement, but that has a fairly negative effect on error messages: #10489. So this fix seems a bit better to me.

Closes https://github.com/astral-sh/uv/issues/10481.


---

_Label `bug` added by @charliermarsh on 2025-01-11 01:59_

---

_@charliermarsh reviewed on 2025-01-11 02:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:13275 on 2025-01-11 02:23_

This isn't _wrong_ but perhaps a bit surprising.

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-11 02:26_

---

_Review requested from @konstin by @charliermarsh on 2025-01-11 02:26_

---

_Review comment by @notatallshaw on `crates/uv/tests/it/pip_compile.rs`:13275 on 2025-01-11 03:06_

This is normal to see for this resolution, it's an artifact of pinning on so many libraries that are a bit old and then trying to resolve them for newer versions of Python.

The correct solution here is that the user (me) shouldn't pin so much, but this is an older set of requirements I use to smoke test new versions of uv, because I know the requirements are challenging. 


---

_@notatallshaw reviewed on 2025-01-11 03:06_

---

_Merged by @charliermarsh on 2025-01-11 03:30_

---

_Closed by @charliermarsh on 2025-01-11 03:30_

---

_Branch deleted on 2025-01-11 03:30_

---

_@BurntSushi reviewed on 2025-01-11 13:06_

I buy this I think. The point of forking is really to address differing markers, but if the markers are the same (and the Python requirement isn't changed), then it seems _fine_ to me that we don't fork in that case.

---
