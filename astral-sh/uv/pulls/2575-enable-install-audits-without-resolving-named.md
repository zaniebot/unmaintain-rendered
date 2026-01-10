```yaml
number: 2575
title: Enable install audits without resolving named requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/bare-iv
created_at: 2024-03-20T20:13:40Z
updated_at: 2024-03-21T03:54:46Z
url: https://github.com/astral-sh/uv/pull/2575
synced_at: 2026-01-10T14:49:08Z
```

# Enable install audits without resolving named requirements

---

_Pull request opened by @charliermarsh on 2024-03-20 20:13_

## Summary

This PR ensures that if a package is already satisfied by the current environment, we don't bother resolving the named requirement.

Part of: https://github.com/astral-sh/uv/issues/313.

## Test Plan

- `cargo run pip install ./scripts/editable-installs/black_editable`
- `cargo run pip install black --verbose`


---

_Marked ready for review by @charliermarsh on 2024-03-20 23:40_

---

_Label `enhancement` added by @charliermarsh on 2024-03-20 23:40_

---

_Merged by @charliermarsh on 2024-03-21 03:54_

---

_Closed by @charliermarsh on 2024-03-21 03:54_

---

_Branch deleted on 2024-03-21 03:54_

---
