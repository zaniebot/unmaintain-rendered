```yaml
number: 5392
title: Validate successful metadata fetch for direct dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/off
created_at: 2024-07-24T02:23:30Z
updated_at: 2024-08-23T15:55:30Z
url: https://github.com/astral-sh/uv/pull/5392
synced_at: 2026-01-10T13:09:50Z
```

# Validate successful metadata fetch for direct dependencies

---

_Pull request opened by @charliermarsh on 2024-07-24 02:23_

## Summary

Prior to this change, the resolver would panic if we ran with `--offline` and `--no-deps` and we had cached metadata for a _package_ (i.e., the versions) but no cached metadata for the _distribution_ (i.e., the specific wheel), since we weren't validating that the returned metadata in the `--no-deps` case was actually successful. (We need metadata, even for `--no-deps`, so that we can validate extras.)

## Test Plan

The added test panics on the previous branch.


---

_Label `bug` added by @charliermarsh on 2024-07-24 02:23_

---

_Merged by @charliermarsh on 2024-07-24 19:44_

---

_Closed by @charliermarsh on 2024-07-24 19:44_

---

_Branch deleted on 2024-07-24 19:44_

---
