```yaml
number: 4156
title: Add incorrect conditional-extra test
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
  - preview
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2024-06-08T01:13:03Z
updated_at: 2024-06-10T12:19:29Z
url: https://github.com/astral-sh/uv/pull/4156
synced_at: 2026-01-12T16:06:04Z
```

# Add incorrect conditional-extra test

---

_@charliermarsh_

_No description provided._

---

_Label `testing` added by @charliermarsh on 2024-06-08 01:13_

---

_Label `preview` added by @charliermarsh on 2024-06-08 01:13_

---

_Merged by @charliermarsh on 2024-06-08 01:24_

---

_Closed by @charliermarsh on 2024-06-08 01:24_

---

_Branch deleted on 2024-06-08 01:24_

---

_@BurntSushi reviewed on 2024-06-10 12:19_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:1150 on 2024-06-10 12:19_

Ah I see, yes, this is the problem.

I think I now finally agree with you that markers need to be put on the edges, not the nodes. Thank you for finding a concrete example. :)

---
