---
number: 1553
title: Wheel names should be URL decoded before validating
type: issue
state: closed
author: ewhauser
labels:
  - bug
assignees: []
created_at: 2024-02-16T23:37:15Z
updated_at: 2024-02-17T00:15:26Z
url: https://github.com/astral-sh/uv/issues/1553
synced_at: 2026-01-10T01:23:07Z
---

# Wheel names should be URL decoded before validating

---

_Issue opened by @ewhauser on 2024-02-16 23:37_

We have some nightly torch dependencies which have encoded URLs:

```
torch @ https://download.pytorch.org/whl/nightly/cu117/torch-2.1.0.dev20230506%2Bcu117-cp311-cp311-linux_x86_64.whl
torchaudio @ https://download.pytorch.org/whl/nightly/cu117/torchaudio-2.1.0.dev20230507%2Bcu117-cp311-cp311-linux_x86_64.whl
torch_triton @ https://download.pytorch.org/whl/nightly/pytorch_triton-2.1.0%2B7d1a95b046-cp311-cp311-linux_x86_64.whl
```

uv fails validation of the wheel name because it doesn't decode the URL first when validating:

```
error: The wheel filename "pytorch_triton-2.1.0%2B7d1a95b046-cp311-cp311-linux_x86_64.whl" has an invalid version part: after parsing 2.1.0, found "%2B7d1a95b046" after it, which is not part of a valid version
```

---

_Comment by @T-256 on 2024-02-16 23:44_

Related https://github.com/astral-sh/uv/issues/1537

---

_Closed by @ewhauser on 2024-02-16 23:48_

---

_Reopened by @ewhauser on 2024-02-16 23:49_

---

_Comment by @charliermarsh on 2024-02-16 23:51_

This looks like it's still a bug on `main`, will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 23:51_

---

_Label `bug` added by @charliermarsh on 2024-02-17 00:04_

---

_Referenced in [astral-sh/uv#1555](../../astral-sh/uv/pulls/1555.md) on 2024-02-17 00:04_

---

_Closed by @charliermarsh on 2024-02-17 00:15_

---
