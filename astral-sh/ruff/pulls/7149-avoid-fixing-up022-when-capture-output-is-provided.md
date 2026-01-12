```yaml
number: 7149
title: "Avoid fixing UP022 when `capture_output` is provided"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/UP022
created_at: 2023-09-05T11:34:01Z
updated_at: 2023-09-05T11:52:33Z
url: https://github.com/astral-sh/ruff/pull/7149
synced_at: 2026-01-12T15:55:23Z
```

# Avoid fixing UP022 when `capture_output` is provided

---

_@charliermarsh_

It would be nice to fix this, although it'd be confusing if `capture_output=False`, and it's such a rare case anyway. (To properly fix this, we need to extend `remove_argument` to support multiple arguments.)

Closes https://github.com/astral-sh/ruff/issues/7126.


---

_Label `bug` added by @charliermarsh on 2023-09-05 11:34_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-05 11:34_

---

_Merged by @charliermarsh on 2023-09-05 11:44_

---

_Closed by @charliermarsh on 2023-09-05 11:44_

---

_Branch deleted on 2023-09-05 11:44_

---

_Comment by @github-actions[bot] on 2023-09-05 11:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
