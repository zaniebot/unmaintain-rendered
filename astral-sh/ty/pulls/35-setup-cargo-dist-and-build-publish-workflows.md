```yaml
number: 35
title: setup cargo-dist and build/publish workflows
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/dist
created_at: 2025-05-05T18:22:11Z
updated_at: 2025-07-08T10:39:12Z
url: https://github.com/astral-sh/ty/pull/35
synced_at: 2026-01-12T15:54:27Z
```

# setup cargo-dist and build/publish workflows

---

_@Gankra_

This is basically an exact copy-paste of the ruff stuff but with `ruff` => `ty` applied pretty blindly.

This likely needs a few iterations to handle the fact that:
* the actions/checkout needs to be recursive
* every path needs to be offset by an extra `./ruff/` segment

---

_@zanieb reviewed on 2025-05-05 18:23_

---

_Review comment by @zanieb on `.github/workflows/build-docker copy.yml`:1 on 2025-05-05 18:23_

Oopsie?

---

_Marked ready for review by @Gankra on 2025-05-05 19:36_

---

_Merged by @Gankra on 2025-05-05 19:44_

---

_Closed by @Gankra on 2025-05-05 19:44_

---

_Branch deleted on 2025-07-08 10:39_

---
