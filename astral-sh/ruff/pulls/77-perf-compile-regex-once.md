```yaml
number: 77
title: "perf: Compile `Regex` once"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/regex-compile
created_at: 2022-09-01T16:43:55Z
updated_at: 2022-09-01T16:51:07Z
url: https://github.com/astral-sh/ruff/pull/77
synced_at: 2026-01-12T06:00:50Z
```

# perf: Compile `Regex` once

---

_Pull request opened by @Stranger6667 on 2022-09-01 16:43_

This PR moves `Regex::new` behind `Lazy`, so each regex effectively only compiles once. Right now it only affects line checks, and this change improves the performance a lot (253.89 ms -> 22.895 ms for a ~1000 lines Python file with many long lines)

---

_Comment by @charliermarsh on 2022-09-01 16:46_

Really smart, thank you, much appreciated.

---

_Comment by @charliermarsh on 2022-09-01 16:47_

(Merging as soon as CI passes.)

---

_Merged by @charliermarsh on 2022-09-01 16:49_

---

_Closed by @charliermarsh on 2022-09-01 16:49_

---

_Branch deleted on 2022-09-01 16:51_

---
