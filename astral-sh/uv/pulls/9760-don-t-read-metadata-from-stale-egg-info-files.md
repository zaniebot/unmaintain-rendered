```yaml
number: 9760
title: "Don't read metadata from stale `.egg-info` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/egg
created_at: 2024-12-10T01:15:41Z
updated_at: 2024-12-10T02:24:44Z
url: https://github.com/astral-sh/uv/pull/9760
synced_at: 2026-01-10T12:00:01Z
```

# Don't read metadata from stale `.egg-info` files

---

_Pull request opened by @charliermarsh on 2024-12-10 01:15_

## Summary

We were reading an `.egg-info` file from the root directory that didn't apply to the root member -- it was for another workspace member. I think this is driven from some idiosyncracies in the `setuptools` setup for that workspace member, but it's still wrong to fail.

This PR adds a few measures to fix this:

1. We validate the `egg-info` filename against the package metadata.
2. We skip, rather than fail, if we see incorrect metadata in an `egg-info` file or similar. This is an optimization anyway; worst case, we try to build the package, then fail there.

Closes https://github.com/astral-sh/uv/issues/9743.


---

_Label `bug` added by @charliermarsh on 2024-12-10 01:15_

---

_Comment by @charliermarsh on 2024-12-10 01:45_

Need to add a test here.

---

_Merged by @charliermarsh on 2024-12-10 02:24_

---

_Closed by @charliermarsh on 2024-12-10 02:24_

---

_Branch deleted on 2024-12-10 02:24_

---
