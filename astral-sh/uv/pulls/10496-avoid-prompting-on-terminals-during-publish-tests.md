```yaml
number: 10496
title: Avoid prompting on terminals during publish tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - testing
assignees: []
merged: true
base: main
head: charlie/pub
created_at: 2025-01-11T13:54:13Z
updated_at: 2025-01-11T14:06:28Z
url: https://github.com/astral-sh/uv/pull/10496
synced_at: 2026-01-10T11:44:52Z
```

# Avoid prompting on terminals during publish tests

---

_Pull request opened by @charliermarsh on 2025-01-11 13:54_

## Summary

Closes https://github.com/astral-sh/uv/issues/10493.

## Test Plan

Run `cargo test --profile fast-build --no-fail-fast -p uv username_password_sources` from a terminal.


---

_Label `bug` added by @charliermarsh on 2025-01-11 13:54_

---

_Label `testing` added by @charliermarsh on 2025-01-11 13:54_

---

_Review requested from @konstin by @charliermarsh on 2025-01-11 13:55_

---

_Merged by @charliermarsh on 2025-01-11 14:06_

---

_Closed by @charliermarsh on 2025-01-11 14:06_

---

_Branch deleted on 2025-01-11 14:06_

---
