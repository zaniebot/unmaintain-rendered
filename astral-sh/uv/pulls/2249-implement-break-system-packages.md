```yaml
number: 2249
title: "Implement `--break-system-packages`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/break-system-packages
created_at: 2024-03-06T20:17:20Z
updated_at: 2024-03-06T20:37:29Z
url: https://github.com/astral-sh/uv/pull/2249
synced_at: 2026-01-10T14:54:43Z
```

# Implement `--break-system-packages`

---

_Pull request opened by @charliermarsh on 2024-03-06 20:17_

## Summary

Per the [`EXTERNALLY-MANAGED`](https://packaging.python.org/en/latest/specifications/externally-managed-environments/) spec, installers SHOULD add a `--break-system-packages` flag to allow users to override the package manager warnings raised by `EXTERNALLY-MANAGED`. This PR adds the flag to comply with the spec, and enable system Python installs on newer versions of certain distributions.

While this flag feels kind of bad, it's not necessarily a change in behavior. We _already_ allow installing into these system distributions -- it's just that `EXTERNALLY-MANAGED` doesn't exist for distributions that were packaged prior to the spec, so we don't run into this problem.

Closes https://github.com/astral-sh/uv/issues/2234.


---

_Label `compatibility` added by @charliermarsh on 2024-03-06 20:17_

---

_Marked ready for review by @charliermarsh on 2024-03-06 20:18_

---

_@zanieb approved on 2024-03-06 20:37_

---

_Merged by @charliermarsh on 2024-03-06 20:37_

---

_Closed by @charliermarsh on 2024-03-06 20:37_

---

_Branch deleted on 2024-03-06 20:37_

---
