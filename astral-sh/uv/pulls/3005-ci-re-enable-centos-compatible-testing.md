```yaml
number: 3005
title: "ci: re-enable centos compatible testing"
type: pull_request
state: merged
author: samypr100
labels:
  - testing
assignees: []
merged: true
base: main
head: rocky
created_at: 2024-04-12T02:15:02Z
updated_at: 2024-04-12T03:56:26Z
url: https://github.com/astral-sh/uv/pull/3005
synced_at: 2026-01-10T14:43:31Z
```

# ci: re-enable centos compatible testing

---

_Pull request opened by @samypr100 on 2024-04-12 02:15_

## Summary

Closes #2915

## Test Plan

Rocky Linux is a viable and more stable alternative to test compatibility with Centos and RHEL systems.

---

_Marked ready for review by @samypr100 on 2024-04-12 02:21_

---

_Comment by @samypr100 on 2024-04-12 02:28_

See new "CentOS" check https://github.com/astral-sh/uv/actions/runs/8655992399/job/23735833179?pr=3005

---

_@charliermarsh reviewed on 2024-04-12 03:16_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:379 on 2024-04-12 03:16_

Should this just be `python on rockylinux`? And should the job be `system-test-rockylinux`? Or would it be standard to still refer to this as CentOS? I'm not really familiar.

---

_@samypr100 reviewed on 2024-04-12 03:46_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:379 on 2024-04-12 03:46_

Makes sense, renamed in dd7b8952752f3ee8da2e8159233ec191c477e35e

Ultimately, testing on Rocky Linux achieves the same effect of testing compatibility with CentOS and RHEL. Generally speaking, RHEL is not gonna make another CentOS release either so this gives us the best of both worlds right now given the state of affairs with RedHat.

---

_Label `testing` added by @charliermarsh on 2024-04-12 03:51_

---

_Comment by @charliermarsh on 2024-04-12 03:51_

Thank you!

---

_Merged by @charliermarsh on 2024-04-12 03:52_

---

_Closed by @charliermarsh on 2024-04-12 03:52_

---

_Branch deleted on 2024-04-12 03:56_

---
