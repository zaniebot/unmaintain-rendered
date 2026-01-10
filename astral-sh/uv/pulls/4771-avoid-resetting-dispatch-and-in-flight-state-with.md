```yaml
number: 4771
title: Avoid resetting dispatch and in-flight state with reinstalls
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/in-flight
created_at: 2024-07-03T14:23:51Z
updated_at: 2024-07-03T14:31:53Z
url: https://github.com/astral-sh/uv/pull/4771
synced_at: 2026-01-10T13:48:28Z
```

# Avoid resetting dispatch and in-flight state with reinstalls

---

_Pull request opened by @charliermarsh on 2024-07-03 14:23_

## Summary

This used to be necessary because we purged the cache in the `InstallPlan` if the user passed `--reinstall`. _However_, we later changed the cache to be append-only.

## Test Plan

I ran through the test plan in https://github.com/astral-sh/uv/pull/933, which includes an integration test and running `uv pip install --reinstall` with:

```text
setuptools
devpi @ https://files.pythonhosted.org/packages/e4/f3/e334eb4dc930ccb677363e1305a3730003d203efbb023329e4b610e4515b/devpi-2.2.0.tar.gz
```


---

_Label `internal` added by @charliermarsh on 2024-07-03 14:23_

---

_Merged by @charliermarsh on 2024-07-03 14:31_

---

_Closed by @charliermarsh on 2024-07-03 14:31_

---

_Branch deleted on 2024-07-03 14:31_

---
