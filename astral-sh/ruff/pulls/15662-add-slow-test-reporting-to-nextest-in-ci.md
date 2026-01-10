```yaml
number: 15662
title: Add slow-test reporting to nextest in CI
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/slow-tests
created_at: 2025-01-21T23:29:59Z
updated_at: 2025-01-22T07:10:55Z
url: https://github.com/astral-sh/ruff/pull/15662
synced_at: 2026-01-10T20:05:43Z
```

# Add slow-test reporting to nextest in CI

---

_Pull request opened by @zanieb on 2025-01-21 23:29_

This is helpful for spotting tests that are running slow. In uv, we use a 10s threshold. Here, it looks like we could use something smaller.

e.g.

<img width="964" alt="Screenshot 2025-01-21 at 6 08 00 PM" src="https://github.com/user-attachments/assets/b32bbc61-9815-4a43-938d-17cd0cf8b0de" />



---

_Label `internal` added by @zanieb on 2025-01-21 23:30_

---

_@zanieb reviewed on 2025-01-22 00:08_

---

_Review comment by @zanieb on `.config/nextest.toml`:15 on 2025-01-22 00:08_

This configuration copied from uv.

---

_@dhruvmanila approved on 2025-01-22 03:53_

Nice!

---

_Merged by @zanieb on 2025-01-22 07:09_

---

_Closed by @zanieb on 2025-01-22 07:09_

---

_Branch deleted on 2025-01-22 07:09_

---

_Label `internal` removed by @MichaReiser on 2025-01-22 07:10_

---

_Label `ci` added by @MichaReiser on 2025-01-22 07:10_

---
