```yaml
number: 887
title: "Use `mold` for linking in CI tests"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/mold
created_at: 2024-01-11T14:59:12Z
updated_at: 2024-01-11T18:28:54Z
url: https://github.com/astral-sh/uv/pull/887
synced_at: 2026-01-12T16:04:15Z
```

# Use `mold` for linking in CI tests

---

_@zanieb_

Derived from https://github.com/astral-sh/puffin/pull/875

This gets us a significant speedup.

I would not read the commits individually. I can squash them but they were used for testing various scenarios.

### Test compile times

Ranges are the lowest and highest I've seen. Huge variability in GitHub Actions runners.

**Before:**
7m 21s - 8m 22s (cold cache)
110s - 120s (warm cache)

**After:**
6m 15s - 7m 05s (cold cache)
57s - 70s (warm cache)

**Improvement:**
4% - 25% (cold cache)
36% - 52% (warm cache)

---

_Marked ready for review by @zanieb on 2024-01-11 18:20_

---

_Comment by @zanieb on 2024-01-11 18:24_

I think this is worth merging separately from #875. It'll be easier to tell if that's worthwhile once this lands.

---

_@charliermarsh approved on 2024-01-11 18:26_

---

_Merged by @zanieb on 2024-01-11 18:28_

---

_Closed by @zanieb on 2024-01-11 18:28_

---

_Branch deleted on 2024-01-11 18:28_

---
