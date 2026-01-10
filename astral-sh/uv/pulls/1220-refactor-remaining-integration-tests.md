```yaml
number: 1220
title: Refactor remaining integration tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/refactor-remaining-integration-tests
created_at: 2024-02-01T09:33:58Z
updated_at: 2024-02-02T17:01:40Z
url: https://github.com/astral-sh/uv/pull/1220
synced_at: 2026-01-10T15:33:24Z
```

# Refactor remaining integration tests

---

_Pull request opened by @konstin on 2024-02-01 09:33_

Mostly a mechanical refactor to use the `puffin_snapshot!` and `TestContext` infrastructure in the add, remove, venv and pip uninstall tests, in preparation for adding programmatic windows testing filters. The is only one remaining usage of `assert_cmd_snapshot!` now in the `puffin_snapshot!` macro. 

---

_Review requested from @charliermarsh by @konstin on 2024-02-01 09:34_

---

_@zanieb approved on 2024-02-02 02:39_

---

_Merged by @konstin on 2024-02-02 09:27_

---

_Closed by @konstin on 2024-02-02 09:27_

---

_Branch deleted on 2024-02-02 09:27_

---

_Label `internal` added by @zanieb on 2024-02-02 17:01_

---
