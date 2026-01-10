```yaml
number: 6717
title: Rename virtual workspace roots to non-project workspace roots
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/leg
created_at: 2024-08-27T19:33:35Z
updated_at: 2024-08-27T21:36:41Z
url: https://github.com/astral-sh/uv/pull/6717
synced_at: 2026-01-10T13:09:51Z
```

# Rename virtual workspace roots to non-project workspace roots

---

_Pull request opened by @charliermarsh on 2024-08-27 19:33_

## Summary

Closes https://github.com/astral-sh/uv/issues/6709.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 19:34_

---

_Label `internal` added by @charliermarsh on 2024-08-27 19:34_

---

_@zanieb reviewed on 2024-08-27 21:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:07_

Do we even need a special message here? The above message looks fine.

---

_@zanieb approved on 2024-08-27 21:07_

---

_@charliermarsh reviewed on 2024-08-27 21:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:14_

We do because there's no way to define a `requires-python` in a non-project workspace :(

---

_@zanieb reviewed on 2024-08-27 21:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:17_

Isn't it just that you define it as a project? I still don't quite see what value the separate message adds, but don't feel strongly since it's just debug info.

---

_@charliermarsh reviewed on 2024-08-27 21:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:19_

I guess we can do that now, since these are "legacy".

---

_@charliermarsh reviewed on 2024-08-27 21:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:20_

(Before it was a problem because there was no way to resolve it.)

---

_@zanieb reviewed on 2024-08-27 21:32_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:3533 on 2024-08-27 21:32_

Should we be suggesting that they make it a project? Should we just automatically migrate them to a project workspace root? (probably best as a follow-up if so)

---

_@zanieb approved on 2024-08-27 21:32_

---

_@zanieb reviewed on 2024-08-27 21:33_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:327 on 2024-08-27 21:33_

I see, thanks!

---

_Merged by @charliermarsh on 2024-08-27 21:36_

---

_Closed by @charliermarsh on 2024-08-27 21:36_

---

_Branch deleted on 2024-08-27 21:36_

---
