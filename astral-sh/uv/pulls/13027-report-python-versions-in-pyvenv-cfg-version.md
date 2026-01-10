```yaml
number: 13027
title: "Report Python versions in `pyvenv.cfg` version mismatch"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - tracing
assignees: []
merged: true
base: main
head: zb/report-versions
created_at: 2025-04-21T21:59:01Z
updated_at: 2025-04-25T18:06:47Z
url: https://github.com/astral-sh/uv/pull/13027
synced_at: 2026-01-10T11:10:40Z
```

# Report Python versions in `pyvenv.cfg` version mismatch

---

_Pull request opened by @zanieb on 2025-04-21 21:59_

When working on #13025 I noticed this message was lacking versions, which seems frustrating if you're debugging things.

I refactored the general `matches_interpreter` utilities that were added in https://github.com/astral-sh/uv/pull/12884 into a more purpose-fit function that returns an `Option` with the versions if there's a mismatch.


---

_Label `error messages` added by @zanieb on 2025-04-21 21:59_

---

_Label `tracing` added by @zanieb on 2025-04-21 21:59_

---

_Review requested from @konstin by @zanieb on 2025-04-21 22:01_

---

_Review requested from @jtfmumm by @zanieb on 2025-04-21 22:01_

---

_@zanieb reviewed on 2025-04-21 22:10_

---

_Review comment by @zanieb on `crates/uv-python/src/environment.rs`:365 on 2025-04-21 22:10_

The name isn't great... open to suggestions.

---

_@jtfmumm reviewed on 2025-04-22 08:23_

---

_Review comment by @jtfmumm on `crates/uv-python/src/environment.rs`:365 on 2025-04-22 08:23_

It's following the pattern of `get...()` or `find...()` where it's a `None` if not found. So maybe
`.find_pyvenv_cfg_version_conflict()` or something like that

---

_@jtfmumm approved on 2025-04-22 08:29_

---

_@konstin approved on 2025-04-22 08:37_

---

_@zanieb reviewed on 2025-04-25 17:32_

---

_Review comment by @zanieb on `crates/uv-python/src/environment.rs`:365 on 2025-04-25 17:32_

I like that

---

_Closed by @zanieb on 2025-04-25 17:58_

---

_Reopened by @zanieb on 2025-04-25 17:58_

---

_Merged by @zanieb on 2025-04-25 18:06_

---

_Closed by @zanieb on 2025-04-25 18:06_

---

_Branch deleted on 2025-04-25 18:06_

---
