```yaml
number: 656
title: Avoid double resolution during source builds
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/double
created_at: 2023-12-15T04:57:03Z
updated_at: 2023-12-15T17:27:18Z
url: https://github.com/astral-sh/uv/pull/656
synced_at: 2026-01-12T16:04:06Z
```

# Avoid double resolution during source builds

---

_@charliermarsh_

## Summary

This PR ensures that we re-use the resolution to install the build dependencies when building a source distribution. Currently, we only pass along the list of requirements, and then use the `Finder` to map each requirement to a distribution. But we already determine the correct distribution when resolving!

Closes https://github.com/astral-sh/puffin/issues/655.


---

_Review requested from @konstin by @charliermarsh on 2023-12-15 04:57_

---

_@konstin approved on 2023-12-15 08:34_

---

_Merged by @charliermarsh on 2023-12-15 17:27_

---

_Closed by @charliermarsh on 2023-12-15 17:27_

---

_Branch deleted on 2023-12-15 17:27_

---
