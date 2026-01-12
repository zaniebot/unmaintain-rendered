```yaml
number: 371
title: Implement C415
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: C415
created_at: 2022-10-09T04:30:17Z
updated_at: 2022-10-09T14:13:06Z
url: https://github.com/astral-sh/ruff/pull/371
synced_at: 2026-01-12T15:55:04Z
```

# Implement C415

---

_@harupy_

#305

---

_@harupy reviewed on 2022-10-09 05:50_

---

_Review comment by @harupy on `src/ast/checks.rs`:1006 on 2022-10-09 05:50_

Tried `val == 1` first, but got this error:

<img width="754" alt="image" src="https://user-images.githubusercontent.com/17039389/194740275-1cb03619-7e4c-492b-bd81-ab5fdc8d5268.png">


---

_@charliermarsh reviewed on 2022-10-09 13:49_

---

_Review comment by @charliermarsh on `src/ast/checks.rs`:1006 on 2022-10-09 13:49_

Would it be safer to create BigInt(1) and compare them as BigInts?

---

_@harupy reviewed on 2022-10-09 14:11_

---

_Review comment by @harupy on `src/ast/checks.rs`:1006 on 2022-10-09 14:11_

Yeah, that would be safer. Fixed :)

---

_Merged by @charliermarsh on 2022-10-09 14:12_

---

_Closed by @charliermarsh on 2022-10-09 14:12_

---

_Comment by @charliermarsh on 2022-10-09 14:13_

Nice work :)

---
