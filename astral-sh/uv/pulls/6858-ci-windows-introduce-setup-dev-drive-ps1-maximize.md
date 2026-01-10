```yaml
number: 6858
title: "ci(windows): Introduce setup-dev-drive.ps1, maximize dev drive usage"
type: pull_request
state: merged
author: fasterthanlime
labels:
  - testing
assignees: []
merged: true
base: main
head: temp
created_at: 2024-08-30T10:05:27Z
updated_at: 2024-08-30T14:18:43Z
url: https://github.com/astral-sh/uv/pull/6858
synced_at: 2026-01-10T12:53:35Z
```

# ci(windows): Introduce setup-dev-drive.ps1, maximize dev drive usage

---

_Pull request opened by @fasterthanlime on 2024-08-30 10:05_

As suggested by @samypr100 on #6680:
https://github.com/astral-sh/uv/pull/6680#issuecomment-2313607984

## Summary

Instead of using `UV_INTERNAL__TEST_DIR`, it simply exports `TEMP` when running Windows jobs.

## Test Plan

I'm going to run this manually under ProcMon on my Windows machine and see where uv writes temp files, hopefully to the dev drive and not `%(LOCAL)APPDATA%` or something.

I'm going to commit a dummy code change and look at build time changes in CI.

---

_Comment by @fasterthanlime on 2024-08-30 10:55_

So: in `uv/tests/common`, there's a bit of code that makes a directory under the _data_ dir. There's a rationale for macOS.

We need to keep exporting the internal var for Windows, I'm afraid, but I think setting `TEMP` there is also a good idea (for other tools), and I've deduplicated the dev-drive setup into its own PowerShell script. Let's see if it still works.

---

_Renamed from "ci(windows): Simply use the TEMP dir" to "ci(windows): Introduce setup-dev-drive.ps1, maximize dev drive usage" by @fasterthanlime on 2024-08-30 11:53_

---

_@zanieb reviewed on 2024-08-30 12:13_

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:1 on 2024-08-30 12:13_

Arguably could belong in `scripts/` but I don't feel strongly

---

_@zanieb reviewed on 2024-08-30 12:14_

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:1 on 2024-08-30 12:14_

(I guess this relies on `GITHUB_ENV` so nevermind)

---

_@zanieb approved on 2024-08-30 12:15_

---

_Label `testing` added by @zanieb on 2024-08-30 12:15_

---

_@samypr100 reviewed on 2024-08-30 12:27_

---

_Review comment by @samypr100 on `.github/workflows/setup-dev-drive.ps1`:1 on 2024-08-30 12:27_

If there's any interest, I also maintain a separate https://github.com/samypr100/setup-dev-drive which could help achieve the same here if we want to cleanup and have more control on the settings per job

---

_Merged by @charliermarsh on 2024-08-30 12:54_

---

_Closed by @charliermarsh on 2024-08-30 12:54_

---

_@samypr100 reviewed on 2024-08-30 13:08_

---

_Review comment by @samypr100 on `.github/workflows/setup-dev-drive.ps1`:19 on 2024-08-30 13:08_

Was `UV_INTERNAL__TEST_DIR` going to be removed as part of this PR? Maybe a testing remnant

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:19 on 2024-08-30 13:10_

I think this is addressed in https://github.com/astral-sh/uv/pull/6858#issuecomment-2320839792

---

_@zanieb reviewed on 2024-08-30 13:10_

---

_@samypr100 reviewed on 2024-08-30 13:11_

---

_Review comment by @samypr100 on `.github/workflows/setup-dev-drive.ps1`:19 on 2024-08-30 13:11_

I see, thanks

---

_@fasterthanlime reviewed on 2024-08-30 14:18_

---

_Review comment by @fasterthanlime on `.github/workflows/setup-dev-drive.ps1`:19 on 2024-08-30 14:18_

it can't be (woops, zanieb got to this first)

https://github.com/astral-sh/uv/pull/6858#issuecomment-2320839792

---
