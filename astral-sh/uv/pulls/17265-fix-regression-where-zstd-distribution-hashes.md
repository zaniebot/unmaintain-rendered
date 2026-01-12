```yaml
number: 17265
title: Fix regression where zstd distribution hashes were not considered valid
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/zstd-wheel
created_at: 2025-12-30T14:21:38Z
updated_at: 2025-12-30T15:24:04Z
url: https://github.com/astral-sh/uv/pull/17265
synced_at: 2026-01-12T16:12:41Z
```

# Fix regression where zstd distribution hashes were not considered valid

---

_@zanieb_

Fixes a regression from https://github.com/astral-sh/uv/pull/17157 as reported in https://github.com/astral-sh/uv/issues/17260

Closes https://github.com/astral-sh/uv/issues/17260
Closes https://github.com/astral-sh/uv/pull/17263

You can see the regression test fail [here](https://github.com/astral-sh/uv/actions/runs/20599629637/job/59162043790?pr=17269) in #17269 which cherry-picks the commit adding tests without the fix.

---

_Marked ready for review by @zanieb on 2025-12-30 14:31_

---

_Review requested from @woodruffw by @zanieb on 2025-12-30 14:31_

---

_@woodruffw approved on 2025-12-30 15:20_

---

_Merged by @zanieb on 2025-12-30 15:24_

---

_Closed by @zanieb on 2025-12-30 15:24_

---

_Branch deleted on 2025-12-30 15:24_

---
