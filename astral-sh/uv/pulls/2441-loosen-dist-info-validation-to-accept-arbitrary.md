```yaml
number: 2441
title: "Loosen `.dist-info` validation to accept arbitrary versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/dist-info
created_at: 2024-03-14T01:39:59Z
updated_at: 2024-03-14T13:04:40Z
url: https://github.com/astral-sh/uv/pull/2441
synced_at: 2026-01-10T14:49:08Z
```

# Loosen `.dist-info` validation to accept arbitrary versions

---

_Pull request opened by @charliermarsh on 2024-03-14 01:39_

## Summary

It turns out that pip does _not_ validate the normalization of the version specifier in the `.dist-info` directory. In particular, it seems that some tools replace the `+` in a local version segment with a `_`.

Closes https://github.com/astral-sh/uv/issues/2424.


---

_Label `compatibility` added by @charliermarsh on 2024-03-14 01:40_

---

_Review requested from @konstin by @charliermarsh on 2024-03-14 01:40_

---

_@konstin approved on 2024-03-14 09:23_

---

_Merged by @charliermarsh on 2024-03-14 13:04_

---

_Closed by @charliermarsh on 2024-03-14 13:04_

---

_Branch deleted on 2024-03-14 13:04_

---
