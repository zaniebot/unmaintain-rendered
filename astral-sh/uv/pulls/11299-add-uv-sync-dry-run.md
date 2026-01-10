```yaml
number: 11299
title: "Add `uv sync --dry-run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/dry-run
created_at: 2025-02-06T21:26:56Z
updated_at: 2025-02-07T12:55:35Z
url: https://github.com/astral-sh/uv/pull/11299
synced_at: 2026-01-10T11:10:35Z
```

# Add `uv sync --dry-run`

---

_Pull request opened by @charliermarsh on 2025-02-06 21:26_

## Summary

Allows users to understand how the environment will change prior to committing.

Closes https://github.com/astral-sh/uv/issues/11282.


---

_Label `enhancement` added by @charliermarsh on 2025-02-06 21:27_

---

_Label `cli` added by @charliermarsh on 2025-02-06 21:27_

---

_Marked ready for review by @charliermarsh on 2025-02-06 21:27_

---

_Review requested from @Gankra by @charliermarsh on 2025-02-06 21:27_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-06 21:27_

---

_@charliermarsh reviewed on 2025-02-06 21:27_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:965 on 2025-02-06 21:27_

I am going to replace this with an enum ~everywhere, but in a separate PR.

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:3098 on 2025-02-06 22:08_

...interesting. i suppose that makes sense

---

_@zanieb reviewed on 2025-02-06 22:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:6531 on 2025-02-06 22:48_

`Would create lock file at: uv.lock`?

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:6531 on 2025-02-06 22:50_

(and subsequently, would update?)

---

_@zanieb reviewed on 2025-02-06 22:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:315 on 2025-02-06 22:51_

Thanks! We should do this ~throughout

---

_@zanieb reviewed on 2025-02-06 22:51_

---

_@zanieb reviewed on 2025-02-06 22:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1017 on 2025-02-06 22:52_

👍 

---

_@zanieb approved on 2025-02-06 22:53_

---

_@charliermarsh reviewed on 2025-02-06 23:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:6531 on 2025-02-06 23:23_

Added, good call. Forgot.

---

_Merged by @charliermarsh on 2025-02-06 23:52_

---

_Closed by @charliermarsh on 2025-02-06 23:52_

---

_Branch deleted on 2025-02-06 23:52_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/mod.rs`:943 on 2025-02-07 12:52_

bless these docs

---

_@Gankra reviewed on 2025-02-07 12:55_

---
