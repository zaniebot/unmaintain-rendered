```yaml
number: 959
title: Enforce modification freshness checks against virtual environment
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/venv-install
created_at: 2024-01-18T04:32:24Z
updated_at: 2024-01-18T20:21:17Z
url: https://github.com/astral-sh/uv/pull/959
synced_at: 2026-01-12T16:04:19Z
```

# Enforce modification freshness checks against virtual environment

---

_@charliermarsh_

## Summary

This PR is like #957, but for validating the virtual environment, rather than the cache. So, if you have a local wheel, and you rebuild it, we'll now correctly uninstall and reinstall it in the virtual environment.


---

_Review requested from @konstin by @charliermarsh on 2024-01-18 04:32_

---

_Label `enhancement` added by @charliermarsh on 2024-01-18 04:32_

---

_Review comment by @konstin on `requirements.in`:1 on 2024-01-18 12:19_

This one and the one below are test files

---

_@konstin approved on 2024-01-18 12:19_

---

_Merged by @charliermarsh on 2024-01-18 20:21_

---

_Closed by @charliermarsh on 2024-01-18 20:21_

---

_Branch deleted on 2024-01-18 20:21_

---
