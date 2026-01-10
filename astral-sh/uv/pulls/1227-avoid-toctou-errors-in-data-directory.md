```yaml
number: 1227
title: Avoid TOCTOU errors in data directory installations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dir-overlap
created_at: 2024-02-01T14:51:18Z
updated_at: 2024-02-01T14:55:30Z
url: https://github.com/astral-sh/uv/pull/1227
synced_at: 2026-01-10T15:33:24Z
```

# Avoid TOCTOU errors in data directory installations

---

_Pull request opened by @charliermarsh on 2024-02-01 14:51_

## Summary

See: https://github.com/astral-sh/puffin/issues/1224

## Test Plan

Ran `python -m scripts.bench --puffin scripts/requirements/compiled/jupyter.txt --min-runs 100 --benchmark install-warm --verbose` several times, which failed eventually on `main` but not on this branch.


---

_Label `bug` added by @charliermarsh on 2024-02-01 14:51_

---

_@konstin approved on 2024-02-01 14:53_

---

_Merged by @charliermarsh on 2024-02-01 14:55_

---

_Closed by @charliermarsh on 2024-02-01 14:55_

---

_Branch deleted on 2024-02-01 14:55_

---
