---
number: 16832
title: Better source-exclude reference docs
type: pull_request
state: closed
author: konstin
labels:
  - documentation
assignees: []
base: main
head: konsti/backend-source-exclude-docs
created_at: 2025-11-24T13:33:55Z
updated_at: 2025-12-09T21:17:19Z
url: https://github.com/astral-sh/uv/pull/16832
synced_at: 2026-01-10T01:26:18Z
---

# Better source-exclude reference docs

---

_Pull request opened by @konstin on 2025-11-24 13:33_

Fixed https://github.com/astral-sh/uv/issues/16821

This is already explained in the guide, but it was missing from the reference docs.

---

_Label `documentation` added by @konstin on 2025-11-24 13:33_

---

_Referenced in [astral-sh/uv#16821](../../astral-sh/uv/issues/16821.md) on 2025-11-24 13:34_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/settings.rs`:75 on 2025-11-24 15:03_

I find this wording a little confusing. Maybe...

> These exclusions are also applied to wheels to ensure that a wheel built from the
> source tree is consistent with a wheel built from the source distribution.

---

_@zanieb reviewed on 2025-11-24 15:03_

---

_Comment by @codspeed-hq[bot] on 2025-11-28 14:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fbackend-source-exclude-docs?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16832 will **not alter performance**

<sub>Comparing <code>konsti/backend-source-exclude-docs</code> (7302c2b) with <code>main</code> (b6686fb)</sub>



### Summary

`✅ 5` untouched  





---

_Merged by @zanieb on 2025-12-09 21:14_

---

_Closed by @zanieb on 2025-12-09 21:14_

---

_Branch deleted on 2025-12-09 21:14_

---

_Referenced in [Homebrew/homebrew-core#257965](../../Homebrew/homebrew-core/pulls/257965.md) on 2025-12-09 23:22_

---
