```yaml
number: 535
title: Preserve seed packages for non-Puffin-created virtualenvs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/seed-packages
created_at: 2023-12-04T04:33:31Z
updated_at: 2023-12-04T14:31:02Z
url: https://github.com/astral-sh/uv/pull/535
synced_at: 2026-01-10T15:44:44Z
```

# Preserve seed packages for non-Puffin-created virtualenvs

---

_Pull request opened by @charliermarsh on 2023-12-04 04:33_

## Summary

This PR modifies the install plan to avoid removing seed packages if the virtual environment was created by anyone other than Puffin.

Closes https://github.com/astral-sh/puffin/issues/414.

## Test Plan

- Ran: `virtualenv .venv`.
- Ran: `cargo run -p puffin-cli -- pip-sync scripts/benchmarks/requirements.txt --verbose --no-cache`.
- Verified that `pip` et al were not removed, and that the logging including a message around preserving seed packages.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-04 04:33_

---

_Review requested from @konstin by @charliermarsh on 2023-12-04 04:33_

---

_Label `bug` added by @charliermarsh on 2023-12-04 04:33_

---

_@konstin approved on 2023-12-04 10:17_

---

_Merged by @charliermarsh on 2023-12-04 14:31_

---

_Closed by @charliermarsh on 2023-12-04 14:31_

---

_Branch deleted on 2023-12-04 14:31_

---
