```yaml
number: 344
title: Improve root message when version solving fails
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/root-message
created_at: 2023-11-06T19:17:07Z
updated_at: 2023-11-06T20:07:51Z
url: https://github.com/astral-sh/uv/pull/344
synced_at: 2026-01-12T16:03:53Z
```

# Improve root message when version solving fails

---

_@zanieb_

Matching description at https://github.com/dart-lang/pub/blob/master/doc/solver.md#linear-error-reporting


---

_@charliermarsh approved on 2023-11-06 19:41_

---

_@charliermarsh approved on 2023-11-06 19:48_

---

_Review comment by @konstin on `crates/puffin-resolver/src/pubgrub/report.rs`:271 on 2023-11-06 19:54_

Do we want to upstream those?

---

_@konstin approved on 2023-11-06 19:54_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:271 on 2023-11-06 20:00_

The `Root` package enum is internal right now, I'm not sure how they represent it.

---

_@zanieb reviewed on 2023-11-06 20:00_

---

_Merged by @zanieb on 2023-11-06 20:07_

---

_Closed by @zanieb on 2023-11-06 20:07_

---

_Branch deleted on 2023-11-06 20:07_

---
