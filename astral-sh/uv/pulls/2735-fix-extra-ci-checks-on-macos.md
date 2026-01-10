```yaml
number: 2735
title: Fix extra CI checks on macOS
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/fix-ci
created_at: 2024-03-30T14:44:25Z
updated_at: 2024-03-30T14:55:36Z
url: https://github.com/astral-sh/uv/pull/2735
synced_at: 2026-01-10T14:49:08Z
```

# Fix extra CI checks on macOS

---

_Pull request opened by @zanieb on 2024-03-30 14:44_

Alternative to https://github.com/astral-sh/uv/pull/2729 since we're having problems with the Homebrew Python.

---

_Label `testing` added by @zanieb on 2024-03-30 14:44_

---

_Comment by @ChannyClaus on 2024-03-30 14:48_

lovely, seems like this might be the same issue i was looking into just now? 
```
Run python3 scripts/check_system_python.py --uv ./uv
INFO: Checking that `pylint` isn't installed.
WARNING: Package(s) not found: pylint
INFO: Installing the package `pylint`.
error: The interpreter at /opt/homebrew/opt/python@3.[12](https://github.com/astral-sh/uv/actions/runs/8488022849/job/23256549997?pr=2636#step:7:13)/Frameworks/Python.framework/Versions/3.12 is externally managed, and indicates the following:
```
(i was confused since the yaml file seems to install 3.8)

---

_Comment by @zanieb on 2024-03-30 14:52_

Yeah sorry about the churn something changed about the runners it seems?

---

_Marked ready for review by @zanieb on 2024-03-30 14:54_

---

_Comment by @ChannyClaus on 2024-03-30 14:55_

> Yeah sorry about the churn something changed about the runners it seems?

no worries - seems like github may not support pinning the runner version/image (https://github.com/actions/runner-images/issues/2894) 😢 

---

_Merged by @zanieb on 2024-03-30 14:55_

---

_Closed by @zanieb on 2024-03-30 14:55_

---

_Branch deleted on 2024-03-30 14:55_

---
