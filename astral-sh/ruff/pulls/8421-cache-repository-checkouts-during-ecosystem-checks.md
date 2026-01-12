```yaml
number: 8421
title: Cache repository checkouts during ecosystem checks
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zanie/eco-cache
created_at: 2023-11-01T19:53:09Z
updated_at: 2023-11-11T16:37:44Z
url: https://github.com/astral-sh/ruff/pull/8421
synced_at: 2026-01-10T23:40:55Z
```

# Cache repository checkouts during ecosystem checks

---

_Pull request opened by @zanieb on 2023-11-01 19:53_

Requires https://github.com/astral-sh/ruff/pull/8420

Cloning the repositories is slow, maybe it'd be faster if they were cached?



---

_Converted to draft by @zanieb on 2023-11-01 19:53_

---

_Label `internal` added by @zanieb on 2023-11-01 20:00_

---

_Comment by @github-actions[bot] on 2023-11-02 15:18_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2023-11-02 16:27_

This costs us ~2123MB of cache space and saves us about 5 minutes of CI time (~30% improvement)

Comparing the following runs, 16min to 11min
https://github.com/astral-sh/ruff/actions/runs/6734403254/job/18305411170
https://github.com/astral-sh/ruff/actions/runs/6727128677/job/18284600514

---

_Comment by @zanieb on 2023-11-11 16:37_

I don't want to give up 2GB of our cache

---

_Closed by @zanieb on 2023-11-11 16:37_

---
