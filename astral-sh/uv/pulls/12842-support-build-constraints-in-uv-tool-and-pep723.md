```yaml
number: 12842
title: "Support build constraints in `uv tool` and PEP723 scripts."
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: tool-build-constraints
created_at: 2025-04-11T19:17:07Z
updated_at: 2025-04-14T13:27:03Z
url: https://github.com/astral-sh/uv/pull/12842
synced_at: 2026-01-12T16:10:24Z
```

# Support build constraints in `uv tool` and PEP723 scripts.

---

_@blueraft_

## Summary

Closes https://github.com/astral-sh/uv/issues/12496.

## Test Plan

`cargo test`


---

_@blueraft reviewed on 2025-04-11 19:18_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/run.rs`:355 on 2025-04-11 19:18_

Wasn't planning on doing this, but seemed like an easy fix..

---

_@blueraft reviewed on 2025-04-11 19:18_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/run.rs`:902 on 2025-04-11 19:18_

This is tracked here https://github.com/astral-sh/uv/issues/12505

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-14 12:30_

---

_@charliermarsh approved on 2025-04-14 13:02_

Nice, this is excellent. Thank you!

---

_Comment by @codspeed-hq[bot] on 2025-04-14 13:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/blueraft%3Atool-build-constraints)

### Merging #12842 will **not alter performance**

<sub>Comparing <code>blueraft:tool-build-constraints</code> (4e50e4c) with <code>main</code> (66df255)</sub>



### Summary

`✅ 14` untouched benchmarks  





---

_Renamed from "Support build constraints in `uv tool` and PEP732 scripts." to "Support build constraints in `uv tool` and PEP723 scripts." by @blueraft on 2025-04-14 13:17_

---

_Merged by @charliermarsh on 2025-04-14 13:26_

---

_Closed by @charliermarsh on 2025-04-14 13:26_

---

_Label `enhancement` added by @charliermarsh on 2025-04-14 13:27_

---
