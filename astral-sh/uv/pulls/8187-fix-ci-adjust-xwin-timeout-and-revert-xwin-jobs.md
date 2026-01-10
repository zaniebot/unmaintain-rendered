```yaml
number: 8187
title: "fix(ci): adjust xwin timeout and revert xwin jobs being disabled"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: xwin-revival
created_at: 2024-10-14T19:43:17Z
updated_at: 2024-10-14T20:17:49Z
url: https://github.com/astral-sh/uv/pull/8187
synced_at: 2026-01-10T12:54:04Z
```

# fix(ci): adjust xwin timeout and revert xwin jobs being disabled

---

_Pull request opened by @samypr100 on 2024-10-14 19:43_

## Summary

Reverts #8181 and #8182.

The fix is in b849f0f, which extends the run timeout to allow xwin to download the Windows SDK files, which can take 10+ minutes.

Closes https://github.com/rust-cross/cargo-xwin/issues/127

## Test Plan

Existing CI should pass.

## Notes

xwin jobs will take a long time the first time due to cache re-warming.


---

_Marked ready for review by @samypr100 on 2024-10-14 20:01_

---

_Comment by @zanieb on 2024-10-14 20:06_

I wonder if we should even be using xwin for the main Clippy job since it's ~as fast with the native setup?

---

_Comment by @zanieb on 2024-10-14 20:06_

Thanks a bunch of identifying this issue!

---

_Comment by @samypr100 on 2024-10-14 20:13_

> I wonder if we should even be using xwin for the main Clippy job since it's ~as fast with the native setup?

I think the gain is more-so on the `windows-latest-xlarge` vs `ubuntu-latest` / costs and the speed that the job takes to start (total job speeds).

The runs perform the same, I mentioned it here https://github.com/astral-sh/uv/pull/3600#issuecomment-2113857365

iirc, I did compare `windows-latest` vs `ubuntu-latest` (not xlarge) and it was about 30s slower. xlarge was about the same for clippy.


---

_@zanieb approved on 2024-10-14 20:14_

👍 

---

_Label `internal` added by @zanieb on 2024-10-14 20:14_

---

_Merged by @zanieb on 2024-10-14 20:14_

---

_Closed by @zanieb on 2024-10-14 20:14_

---

_Branch deleted on 2024-10-14 20:17_

---
