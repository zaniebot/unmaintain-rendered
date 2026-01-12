```yaml
number: 15884
title: Ignore inferred conflict marker when determining if lockfile is up-to-date
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/lock-conflict-transitive
created_at: 2025-09-16T02:30:10Z
updated_at: 2025-09-16T02:38:02Z
url: https://github.com/astral-sh/uv/pull/15884
synced_at: 2026-01-12T16:12:00Z
```

# Ignore inferred conflict marker when determining if lockfile is up-to-date

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/15869

---

_Label `bug` added by @zanieb on 2025-09-16 02:30_

---

_@zanieb reviewed on 2025-09-16 02:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:11058 on 2025-09-16 02:30_

Unfortunately this regresses the error message, I'll need to investigate that further.

---

_@zanieb reviewed on 2025-09-16 02:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:11005 on 2025-09-16 02:31_

You can see the failure from the issue in https://github.com/astral-sh/uv/pull/15884/commits/44522f0d0b9e71cc35d5b566d14c5ebd986e1b23

---

_@zanieb reviewed on 2025-09-16 02:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:11058 on 2025-09-16 02:38_

Presumably the problem is that we're installing from the lockfile instead of a fresh resolution, which drops this information.

I think there are a few options here

1. Read the pyproject.toml to determine if the conflict is transitive
2. Change the encoding in the lockfile to avoid flattening nested groups
3. Add a marker to the lockfile to indicate a transitive conflict is present
4. Drop this special-case entirely

I sort of argue for (4), while we might want (2) or (3) in the future, they seem like a fair bit of work and the improvement in the error message is quite minor.

---
