```yaml
number: 2542
title: "Add `print_stdout` and `print_stderr` to Clippy enforcement"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/print
created_at: 2023-02-03T14:36:12Z
updated_at: 2023-02-03T16:13:48Z
url: https://github.com/astral-sh/ruff/pull/2542
synced_at: 2026-01-12T15:55:08Z
```

# Add `print_stdout` and `print_stderr` to Clippy enforcement

---

_@charliermarsh_

_No description provided._

---

_@charliermarsh reviewed on 2023-02-03 14:37_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/converter.rs`:70 on 2023-02-03 14:37_

Obviously a little absurd to add these everywhere, but I'd really like to be defensive about this stuff, and we print so rarely that it doesn't bother me to have to guard like this. (The `{` and `}` is required since otherwise the macro gets expanded and the `allow` is ignored.)

---

_Merged by @charliermarsh on 2023-02-03 16:13_

---

_Closed by @charliermarsh on 2023-02-03 16:13_

---

_Branch deleted on 2023-02-03 16:13_

---
