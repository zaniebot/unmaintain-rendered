```yaml
number: 11875
title: Make interpreter caching robust to OS upgrades
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2025-03-01T03:40:08Z
updated_at: 2025-03-02T01:36:40Z
url: https://github.com/astral-sh/uv/pull/11875
synced_at: 2026-01-10T11:10:39Z
```

# Make interpreter caching robust to OS upgrades

---

_Pull request opened by @charliermarsh on 2025-03-01 03:40_

## Summary

In. https://github.com/astral-sh/uv/issues/11857, we had a case of a user that was seeing incorrect resolution results after upgrading to a newer version of macOS, since we retained cache information about the interpreter. This PR adds the OS name and version to the cache key for the interpreter. This seems to be extremely cheap, and it's nice to make this robust so that users don't run into the same confusion in the future.

Closes https://github.com/astral-sh/uv/issues/11857.


---

_Review requested from @zanieb by @charliermarsh on 2025-03-01 03:40_

---

_Review requested from @konstin by @charliermarsh on 2025-03-01 03:40_

---

_Label `bug` added by @charliermarsh on 2025-03-01 03:40_

---

_Marked ready for review by @charliermarsh on 2025-03-01 03:40_

---

_@charliermarsh reviewed on 2025-03-01 03:41_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:855 on 2025-03-01 03:41_

I could also use a SHA of these things; either way. This looks like:

![Screenshot 2025-02-28 at 10 41 20 PM](https://github.com/user-attachments/assets/d2f933ea-71e3-43b4-8997-01d29eaf2fb0)


---

_@charliermarsh reviewed on 2025-03-01 03:41_

---

_Review comment by @charliermarsh on `crates/uv-python/Cargo.toml`:54 on 2025-03-01 03:41_

(We already depend on this.)

---

_@zanieb reviewed on 2025-03-01 16:27_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:855 on 2025-03-01 16:27_

Do we need to update the comment above this too?

---

_@zanieb approved on 2025-03-01 16:27_

Into it. A hash might make sense — but I don't feel strongly.

---

_Merged by @charliermarsh on 2025-03-02 01:36_

---

_Closed by @charliermarsh on 2025-03-02 01:36_

---

_Branch deleted on 2025-03-02 01:36_

---
