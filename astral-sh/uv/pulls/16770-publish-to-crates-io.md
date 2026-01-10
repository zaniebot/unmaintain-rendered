```yaml
number: 16770
title: "Publish to `crates.io`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/crates
created_at: 2025-11-18T18:02:10Z
updated_at: 2025-11-21T00:22:41Z
url: https://github.com/astral-sh/uv/pull/16770
synced_at: 2026-01-10T05:58:11Z
```

# Publish to `crates.io`

---

_Pull request opened by @zanieb on 2025-11-18 18:02_

_No description provided._

---

_Review requested from @Gankra by @zanieb on 2025-11-20 17:28_

---

_Marked ready for review by @zanieb on 2025-11-20 17:29_

---

_Renamed from "Prepare library crates for publish" to "Publish to `crates.io`" by @zanieb on 2025-11-20 17:52_

---

_Review comment by @Gankra on `scripts/bump-workspace-crate-versions.py`:68 on 2025-11-20 18:08_

This is a script a human would run and put up as a PR, and so any issues where an ecosystem package happens to have this version would be fine..? Oh nice you set the replace limit to 1, perfect.

---

_Review comment by @Gankra on `scripts/bump-workspace-crate-versions.py`:68 on 2025-11-20 18:09_

This comment brought to you by "I'm lowkey excited I've been dogfooding python enough to recognize basic things like this".

---

_@Gankra approved on 2025-11-20 18:10_

All looks reasonable

---

_@zanieb reviewed on 2025-11-20 18:21_

---

_Review comment by @zanieb on `scripts/bump-workspace-crate-versions.py`:68 on 2025-11-20 18:21_

Yeah this should be pretty safe though of course it's janky. Unfortunately using the Python tomli-w library breaks the manifest in serious ways.

---

_Comment by @codspeed-hq[bot] on 2025-11-20 18:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fcrates?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16770 will **not alter performance**

<sub>Comparing <code>zb/crates</code> (0968891) with <code>main</code> (5eda329)</sub>



### Summary

`✅ 6` untouched  





---

_@github-advanced-security[bot] reviewed on 2025-11-20 20:05_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/ci.yml`:195 on 2025-11-20 20:05_

prefer trusted publishing for authentication

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/182)

---

_Merged by @zanieb on 2025-11-20 21:26_

---

_Closed by @zanieb on 2025-11-20 21:26_

---

_Branch deleted on 2025-11-20 21:26_

---

_@homeworkprod reviewed on 2025-11-21 00:22_

---

_Review comment by @homeworkprod on `scripts/bump-workspace-crate-versions.py`:68 on 2025-11-21 00:22_

@Gankra: Are you referring to recognizing the meaning of the "magic number" `1` here?

Passing it as keyword argument would make more obvious: `contents.replace(old, new, count=1)`

---
