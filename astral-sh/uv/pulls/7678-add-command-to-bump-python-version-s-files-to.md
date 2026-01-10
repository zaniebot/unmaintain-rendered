```yaml
number: 7678
title: Add command to bump .python-version(s) files to latest patch version
type: pull_request
state: closed
author: tfsingh
labels: []
assignees: []
base: main
head: tfsingh/patch
created_at: 2024-09-25T05:21:50Z
updated_at: 2024-12-02T05:32:01Z
url: https://github.com/astral-sh/uv/pull/7678
synced_at: 2026-01-10T11:59:59Z
```

# Add command to bump .python-version(s) files to latest patch version

---

_Pull request opened by @tfsingh on 2024-09-25 05:21_

This PR adds a command (```uv python patch```) which bumps the distributions listed in .python-versions(s) files to the latest patch version available.

The general strategy for finding the latest patch is to
1. check downloads
2. fallback to system versions

Note that tests may not be deterministic as patches are added — this has been documented. I picked Python versions that I believe should be installed on CI runners (based on this):

<img width="437" alt="image" src="https://github.com/user-attachments/assets/61995225-1beb-49a4-bc1d-c37c050ff862">


Closes #7569.

---

_Comment by @zanieb on 2024-09-25 14:38_

I think I'd spell this as a flag for `uv python pin` like `--upgrade`. 

---

_Marked ready for review by @tfsingh on 2024-09-26 01:18_

---

_Comment by @tfsingh on 2024-09-26 01:40_

@zanieb @charliermarsh Any thoughts on how to best address the discrepancy in the Windows runners installing Python `3.8.10`, while Ubuntu/Mac install `3.8.12`? Looks like I shouldn't modify the CI script to specify a patch:

> We do not test with Python patch versions on Windows, so we can use `setup-python` instead of our bootstrapping code, this is much faster on the extremely slow GitHub Windows runners.
      
My thought is to use a cfg! clause to skip the test on windows. I checked if we could potentially modify the version used by mac/ubuntu (`3.8.12`) to `3.8.10`, but the runners don't seem to install `3.8.10` on Windows every time (see [this](https://github.com/astral-sh/uv/actions/runs/11043536499/job/30677981665) job, where `3.10.11` was installed).

---

_Comment by @zanieb on 2024-09-26 12:11_

I would mark the test as requiring the `python-patch` feature

---

_Comment by @tfsingh on 2024-09-26 17:23_

Thanks for the tip! Should be ready for a pass (failure seems unrelated)

---

_Closed by @tfsingh on 2024-12-02 05:32_

---
