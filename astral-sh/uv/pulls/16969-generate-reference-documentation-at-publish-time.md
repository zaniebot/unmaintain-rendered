---
number: 16969
title: Generate reference documentation at publish-time and the JSON schema at release-time
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/gen-all
created_at: 2025-12-03T14:58:29Z
updated_at: 2025-12-08T12:38:28Z
url: https://github.com/astral-sh/uv/pull/16969
synced_at: 2026-01-10T01:26:19Z
---

# Generate reference documentation at publish-time and the JSON schema at release-time

---

_Pull request opened by @zanieb on 2025-12-03 14:58_

It'd be nice to avoid churn for contributors. This is a pretty frequent cause of CI failures and I don't think we really need to have the reference documentation committed. 

---

_Comment by @codspeed-hq[bot] on 2025-12-03 15:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fgen-all?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16969 will **not alter performance**

<sub>Comparing <code>zb/gen-all</code> (ce29531) with <code>main</code> (28a8194)</sub>



### Summary

`✅ 5` untouched  





---

_@zanieb reviewed on 2025-12-03 15:27_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:222 on 2025-12-03 15:27_

We still retain / commit the sysconfig metadata

---

_Marked ready for review by @zanieb on 2025-12-03 15:28_

---

_Review requested from @konstin by @zanieb on 2025-12-04 15:34_

---

_Label `internal` added by @konstin on 2025-12-08 11:01_

---

_Comment by @konstin on 2025-12-08 11:04_

It would be great to have a way to check the generated documentation - we had incorrect rendering before - unfortunately, having the files added to git didn't help with that really.

---

_@konstin approved on 2025-12-08 11:04_

---

_Comment by @zanieb on 2025-12-08 12:12_

Yeah I think if we're uncertain about the rendering we'll just need to run mkdocs locally anyway, unless we add preview environments for documentation.

---

_Merged by @zanieb on 2025-12-08 12:31_

---

_Closed by @zanieb on 2025-12-08 12:31_

---

_Branch deleted on 2025-12-08 12:31_

---

_Referenced in [astral-sh/uv#17031](../../astral-sh/uv/pulls/17031.md) on 2025-12-08 15:35_

---
