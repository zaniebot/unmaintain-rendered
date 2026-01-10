```yaml
number: 6898
title: "Support `uv build --wheel` from source distributions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/sdist-from-wheel
created_at: 2024-08-31T18:31:31Z
updated_at: 2024-09-04T15:30:33Z
url: https://github.com/astral-sh/uv/pull/6898
synced_at: 2026-01-10T12:53:36Z
```

# Support `uv build --wheel` from source distributions

---

_Pull request opened by @charliermarsh on 2024-08-31 18:31_

## Summary

This PR allows users to run `uv build --wheel ./path/to/source.tar.gz` to build a wheel from a source distribution. This is also the default behavior if you run `uv build ./path/to/source.tar.gz`. If you pass `--sdist`, we error.

---

_Label `enhancement` added by @charliermarsh on 2024-08-31 18:31_

---

_Label `cli` added by @charliermarsh on 2024-08-31 18:31_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-31 18:38_

---

_Review requested from @konstin by @charliermarsh on 2024-08-31 18:38_

---

_@konstin approved on 2024-09-02 08:43_

---

_@zanieb approved on 2024-09-03 19:23_

---

_Merged by @charliermarsh on 2024-09-04 15:30_

---

_Closed by @charliermarsh on 2024-09-04 15:30_

---

_Branch deleted on 2024-09-04 15:30_

---
