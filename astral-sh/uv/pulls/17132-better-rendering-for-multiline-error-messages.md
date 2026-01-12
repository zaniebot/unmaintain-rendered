```yaml
number: 17132
title: Better rendering for multiline error messages
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/multiline-errors
created_at: 2025-12-15T13:06:45Z
updated_at: 2025-12-15T16:31:26Z
url: https://github.com/astral-sh/uv/pull/17132
synced_at: 2026-01-12T16:12:37Z
```

# Better rendering for multiline error messages

---

_@konstin_

Split out from https://github.com/astral-sh/uv/pull/17110

Indent multiline error messages properly, and add a test with a multiline context and a context below since that combination isn't captured atm.


---

_Review requested from @zanieb by @konstin on 2025-12-15 13:06_

---

_Label `enhancement` added by @konstin on 2025-12-15 13:06_

---

_@zanieb approved on 2025-12-15 14:17_

---

_@konstin reviewed on 2025-12-15 15:18_

---

_Review comment by @konstin on `crates/uv-warnings/src/lib.rs`:147 on 2025-12-15 15:18_

A lot of IDEs are configured to strip trailing whitespace, which will strip this. Could we instead avoid indenting empty lines? This would be more consistent with ruff and rustfmt, which both don't indent empty lines.

---

_@zanieb reviewed on 2025-12-15 15:20_

---

_Review comment by @zanieb on `crates/uv-warnings/src/lib.rs`:147 on 2025-12-15 15:20_

I think that's fine with me too.

---

_Comment by @codspeed-hq[bot] on 2025-12-15 16:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fmultiline-errors?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #17132 will **not alter performance**

<sub>Comparing <code>konsti/multiline-errors</code> (cdfe1cd) with <code>main</code> (a768a9d)</sub>



### Summary

`✅ 5` untouched  





---

_Merged by @konstin on 2025-12-15 16:29_

---

_Closed by @konstin on 2025-12-15 16:29_

---

_Branch deleted on 2025-12-15 16:29_

---

_Comment by @codspeed-hq[bot] on 2025-12-15 16:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_ERROR_PERFORMANCE_REPORT_COMMENT__ -->
## Unable to generate the flame graphs

The performance report has correctly been generated, but there was an internal error while generating the flame graphs for this run. We're working on fixing the issue. Feel free to contact us on Discord or at [support@codspeed.io](mailto:support@codspeed.io) if the issue persists.

---
