```yaml
number: 14489
title: "Add `exclude-newer-package`"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
assignees: []
merged: true
base: main
head: zb/exclude-newer-package
created_at: 2025-07-07T16:29:19Z
updated_at: 2025-07-29T22:00:27Z
url: https://github.com/astral-sh/uv/pull/14489
synced_at: 2026-01-12T16:11:14Z
```

# Add `exclude-newer-package`

---

_@zanieb_

Adds `exclude-newer-package = { package = timestamp, ... } ` and `--exclude-newer-package package=timestamp`. These take precedence over `exclude-newer` for a given package.

This does need to be serialized to the lockfile, so the revision is bumped to 3. I tested a previous version and we can read a lockfile with this information just fine.

Closes https://github.com/astral-sh/uv/issues/14394

---

_Label `configuration` added by @zanieb on 2025-07-07 16:29_

---

_Marked ready for review by @zanieb on 2025-07-22 14:00_

---

_Review requested from @konstin by @zanieb on 2025-07-25 19:23_

---

_Review requested from @jtfmumm by @zanieb on 2025-07-25 19:24_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:298 on 2025-07-28 09:54_

Why are we doing this loop where we unpack an `ExcludeNewer` to eventually build an `ExcludeNewer` again? 

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:1285 on 2025-07-28 10:07_

Should we `Box` instead?

---

_@konstin reviewed on 2025-07-28 10:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:1285 on 2025-07-28 12:41_

I don't think it's worth it, it's a small size difference and we don't construct many of these enums.

---

_@zanieb reviewed on 2025-07-28 12:41_

---

_@zanieb reviewed on 2025-07-28 12:42_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:298 on 2025-07-28 12:42_

I'll look, there was something weird there but maybe it's since been resolved.

---

_@zanieb reviewed on 2025-07-28 13:25_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:298 on 2025-07-28 13:25_

Yeah... because `ResolverOptions` is used to deserialize from the lockfile, so they need to be decomposed

---

_Merged by @zanieb on 2025-07-29 22:00_

---

_Closed by @zanieb on 2025-07-29 22:00_

---

_Branch deleted on 2025-07-29 22:00_

---
