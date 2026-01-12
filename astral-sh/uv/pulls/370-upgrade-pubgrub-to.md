```yaml
number: 370
title: "Upgrade PubGrub to `02a19f72d636119fb3f826a922122e21da69cfa5`"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/pubgrub
created_at: 2023-11-09T00:21:13Z
updated_at: 2023-11-19T17:25:11Z
url: https://github.com/astral-sh/uv/pull/370
synced_at: 2026-01-12T16:03:54Z
```

# Upgrade PubGrub to `02a19f72d636119fb3f826a922122e21da69cfa5`

---

_@charliermarsh_

_No description provided._

---

_Converted to draft by @charliermarsh on 2023-11-09 00:21_

---

_Assigned to @zanieb by @zanieb on 2023-11-09 18:22_

---

_Comment by @zanieb on 2023-11-09 21:40_

Requires a newer Rust version for https://github.com/rust-lang/rust/issues/91611 which was very recently stabilized.


---

_Comment by @konstin on 2023-11-10 09:21_

So pubgrub currently only works on nightly?

---

_Comment by @zanieb on 2023-11-19 17:25_

This is just an experimental branch of PubGrub which is not in `dev` because the needed interface was not yet released in stable. I think this is resolved now and either Jacob or I will take this work to completion.

---

_Closed by @zanieb on 2023-11-19 17:25_

---
