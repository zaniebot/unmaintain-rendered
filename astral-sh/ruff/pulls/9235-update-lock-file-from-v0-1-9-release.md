```yaml
number: 9235
title: update lock file from v0.1.9 release
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/update-lock-file
created_at: 2023-12-21T19:44:19Z
updated_at: 2023-12-22T12:51:57Z
url: https://github.com/astral-sh/ruff/pull/9235
synced_at: 2026-01-10T23:07:18Z
```

# update lock file from v0.1.9 release

---

_Pull request opened by @BurntSushi on 2023-12-21 19:44_

This should have been done before the actual release, so we add another step to `CONTRIBUTING.md` to make sure it gets done in the future.

This doesn't fix https://github.com/astral-sh/ruff/issues/9234 completely, but it's a step in the right direction.

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-21 19:44_

---

_Review requested from @zanieb by @BurntSushi on 2023-12-21 19:44_

---

_@charliermarsh approved on 2023-12-21 19:45_

Thanks!

---

_Comment by @github-actions[bot] on 2023-12-21 20:20_

<!-- generated-comment ecosystem -->
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

_Merged by @charliermarsh on 2023-12-21 20:47_

---

_Closed by @charliermarsh on 2023-12-21 20:47_

---

_Branch deleted on 2023-12-21 20:47_

---

_Label `internal` added by @BurntSushi on 2023-12-21 21:13_

---

_Comment by @WhyNotHugo on 2023-12-22 12:51_

Ideally, CI would use `--locked` so it would fail if the lockfile is ever out of date.

---
