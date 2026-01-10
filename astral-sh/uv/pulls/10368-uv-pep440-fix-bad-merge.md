```yaml
number: 10368
title: "uv-pep440: fix bad merge"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/pep440-fix-bad-merge
created_at: 2025-01-07T14:59:43Z
updated_at: 2025-01-07T15:11:42Z
url: https://github.com/astral-sh/uv/pull/10368
synced_at: 2026-01-10T11:44:45Z
```

# uv-pep440: fix bad merge

---

_Pull request opened by @BurntSushi on 2025-01-07 14:59_

This happened as a result of #10345 and #10362 being merged
independently. The latter used the old `Version::release` API, but the
former changed the `Version::release` API. This PR tweaks the new test
to use the new API (i.e., force a deref on the proxy type).


---

_@charliermarsh approved on 2025-01-07 15:00_

---

_Merged by @BurntSushi on 2025-01-07 15:11_

---

_Closed by @BurntSushi on 2025-01-07 15:11_

---

_Branch deleted on 2025-01-07 15:11_

---
