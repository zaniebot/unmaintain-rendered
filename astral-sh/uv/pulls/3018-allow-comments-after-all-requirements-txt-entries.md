```yaml
number: 3018
title: Allow comments after all requirements.txt entries
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2024-04-12T19:03:48Z
updated_at: 2024-04-12T19:56:58Z
url: https://github.com/astral-sh/uv/pull/3018
synced_at: 2026-01-10T14:43:31Z
```

# Allow comments after all requirements.txt entries

---

_Pull request opened by @charliermarsh on 2024-04-12 19:03_

## Summary

I'm surprised we haven't hit this before, but apparently we don't allow comments after `--index-url`, `-e` entries, etc., in the requirements.txt parser.

Closes #3011.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2024-04-12 19:03_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:805 on 2024-04-12 19:04_

Not necessary, I think, because the next step after parsing an entry is to eat the trailing line.

---

_@charliermarsh reviewed on 2024-04-12 19:04_

---

_@charliermarsh reviewed on 2024-04-12 19:04_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:584 on 2024-04-12 19:04_

Not necessary, I think, because the next step after parsing an entry is to eat the trailing line.

---

_@charliermarsh reviewed on 2024-04-12 19:04_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:593 on 2024-04-12 19:04_

Not necessary, I think, because the next step after parsing an entry is to eat the trailing line.

---

_Label `bug` added by @charliermarsh on 2024-04-12 19:04_

---

_@zanieb approved on 2024-04-12 19:53_

---

_Merged by @charliermarsh on 2024-04-12 19:56_

---

_Closed by @charliermarsh on 2024-04-12 19:56_

---

_Branch deleted on 2024-04-12 19:56_

---
