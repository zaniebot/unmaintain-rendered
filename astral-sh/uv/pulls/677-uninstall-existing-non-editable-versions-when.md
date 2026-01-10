```yaml
number: 677
title: Uninstall existing non-editable versions when installing editable requirements
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/migrate-to-editable
created_at: 2023-12-18T02:14:58Z
updated_at: 2023-12-18T09:28:15Z
url: https://github.com/astral-sh/uv/pull/677
synced_at: 2026-01-10T15:44:44Z
```

# Uninstall existing non-editable versions when installing editable requirements

---

_Pull request opened by @charliermarsh on 2023-12-18 02:14_

If you install `black` from PyPI, then `-e ../black`, we need to uninstall the existing `black`. This sounds simple, but that in turn requires that we _know_ `-e ../black` maps to the package `black`, so that we can mark it for uninstallation in the install plan. This, in turn, means that we need to build editable dependencies prior to the install plan.

This is just a bunch of reorganization to fix that specific bug (installing multiple versions of `black` if you run through the above workflow): we now run through the list of editables upfront, mark those that are already installed, build those that aren't, and then ensure that `InstallPlan` correctly removes those that need to be removed, etc.

Closes #676.

---

_Review requested from @konstin by @charliermarsh on 2023-12-18 02:17_

---

_Label `bug` added by @charliermarsh on 2023-12-18 02:17_

---

_@konstin approved on 2023-12-18 09:16_

---

_Closed by @konstin on 2023-12-18 09:28_

---
