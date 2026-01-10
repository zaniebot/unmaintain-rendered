```yaml
number: 7274
title: Bump version to 0.4.9
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/049
created_at: 2024-09-10T20:18:51Z
updated_at: 2024-09-10T21:44:40Z
url: https://github.com/astral-sh/uv/pull/7274
synced_at: 2026-01-10T12:53:43Z
```

# Bump version to 0.4.9

---

_Pull request opened by @zanieb on 2024-09-10 20:18_

_No description provided._

---

_Label `releases` added by @zanieb on 2024-09-10 20:18_

---

_Comment by @zanieb on 2024-09-10 21:12_

Reverting https://github.com/astral-sh/uv/pull/5927 because it changed a bunch of versions and CI is failing.

---

_Comment by @codspeed-hq[bot] on 2024-09-10 21:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/049)

### Merging #7274 will **not alter performance**

<sub>Comparing <code>zb/049</code> (a454949) with <code>main</code> (3f011f3)</sub>



### Summary

`✅ 14` untouched benchmarks






---

_@charliermarsh approved on 2024-09-10 21:25_

---

_@charliermarsh reviewed on 2024-09-10 21:25_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:1527 on 2024-09-10 21:25_

Was this intended to be removed?

---

_@zanieb reviewed on 2024-09-10 21:25_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1527 on 2024-09-10 21:25_

Yeah, but it's restored now that https://github.com/astral-sh/uv/pull/7276 is merged

---

_Comment by @zanieb on 2024-09-10 21:26_

https://github.com/astral-sh/uv/pull/7276 reverted the lock changes so I've rebased onto `main` and dropped the revert of https://github.com/astral-sh/uv/pull/5927

---

_Merged by @zanieb on 2024-09-10 21:44_

---

_Closed by @zanieb on 2024-09-10 21:44_

---

_Branch deleted on 2024-09-10 21:44_

---
