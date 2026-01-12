```yaml
number: 22506
title: Set priority for prek to enable concurrent execution
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/priority
created_at: 2026-01-11T18:34:59Z
updated_at: 2026-01-11T19:21:40Z
url: https://github.com/astral-sh/ruff/pull/22506
synced_at: 2026-01-12T02:26:21Z
```

# Set priority for prek to enable concurrent execution

---

_Pull request opened by @charliermarsh on 2026-01-11 18:34_

## Summary

prek allows you to set priorities, and can run tasks of the same priority concurrently (e.g., we can run Ruff's Python formatting and `cargo fmt` at the same time). On my machine, this takes `uvx prek run -a` from 19.4s to 5.0s (~a 4x speed-up).


---

_Label `internal` added by @charliermarsh on 2026-01-11 18:35_

---

_Marked ready for review by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-11 18:35_

---

_@AlexWaygood approved on 2026-01-11 18:39_

---

_Review requested from @carljm by @charliermarsh on 2026-01-11 18:44_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-11 18:44_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-11 18:44_

---

_Merged by @charliermarsh on 2026-01-11 19:21_

---

_Closed by @charliermarsh on 2026-01-11 19:21_

---

_Branch deleted on 2026-01-11 19:21_

---
