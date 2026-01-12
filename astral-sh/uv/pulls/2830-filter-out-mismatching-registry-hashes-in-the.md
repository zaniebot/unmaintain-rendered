```yaml
number: 2830
title: Filter out mismatching registry hashes in the resolver
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/check-hashes-ii
created_at: 2024-04-05T03:00:51Z
updated_at: 2024-04-12T13:40:08Z
url: https://github.com/astral-sh/uv/pull/2830
synced_at: 2026-01-12T16:05:14Z
```

# Filter out mismatching registry hashes in the resolver

---

_@charliermarsh_

## Summary

This PR adds hash filtering to the `VersionMap` and `FlatIndex`. In other words, packages that don't match one of the provided hashes are now marked as incompatible.

This isn't quite the same as pip. In pip, if none of the versions match, pip will return the distributions that lack hashes, and choose the best match from those distributions. We may want to enable this.


---

_Renamed from "Filter out mismatching hashes in the resolver" to "Filter out mismatching registry hashes in the resolver" by @charliermarsh on 2024-04-05 03:01_

---

_Comment by @charliermarsh on 2024-04-12 13:40_

This was superseded by other work.

---

_Closed by @charliermarsh on 2024-04-12 13:40_

---
