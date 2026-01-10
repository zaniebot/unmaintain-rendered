```yaml
number: 15019
title: Address linter findings in build-binaries.yml
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/build-binaries-fixes
created_at: 2025-08-01T20:29:38Z
updated_at: 2025-08-06T16:00:53Z
url: https://github.com/astral-sh/uv/pull/15019
synced_at: 2026-01-10T06:44:33Z
```

# Address linter findings in build-binaries.yml

---

_Pull request opened by @woodruffw on 2025-08-01 20:29_

## Summary

Addresses (mostly minor) findings in `build-binaries.yml`. This is 99% replacing template expansions with shell-interpolated variables, plus adding `persist-credentials: false` to every checkout.

## Test Plan

See what happens in CI.

---

_Review requested from @zanieb by @woodruffw on 2025-08-01 20:29_

---

_Assigned to @woodruffw by @woodruffw on 2025-08-01 20:29_

---

_Label `internal` added by @woodruffw on 2025-08-01 20:29_

---

_Comment by @zanieb on 2025-08-01 20:46_

```
  /home/runner/work/_actions/uraimo/run-on-arch-action/d94c13912ea685de38fccc1109385b83fd79427d/src/run-on-arch-commands.sh: line 3: PACKAGE_NAME: unbound variable
```

---

_Comment by @woodruffw on 2025-08-01 20:57_

Oh huh, the `uraimo/run-on-arch-action` stuff is abstracted a bit. I'll be able to fix that tonight...

---

_Comment by @woodruffw on 2025-08-04 16:03_

This is good for another look -- one thing this made me realize is that we could probably drop the `addnab/docker-run-action` dependency, but I can follow up on that later.

---

_Review requested from @konstin by @zanieb on 2025-08-04 21:21_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:43 on 2025-08-06 08:41_

Is there a difference between `permissions: {}` and `permissions: `? Asking cause we usually do the second one.

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:692 on 2025-08-06 08:53_

nit: defined by the global env

---

_@konstin approved on 2025-08-06 08:53_

---

_Renamed from "chore(ci): address findings in build-binaries.yml" to "Address linter findings in build-binaries.yml" by @konstin on 2025-08-06 12:11_

---

_@woodruffw reviewed on 2025-08-06 14:50_

---

_Review comment by @woodruffw on `.github/workflows/build-binaries.yml`:43 on 2025-08-06 14:50_

I _think_ there's no difference between the two, assuming by `permissions:` you mean without a trailing block mapping. The GitHub docs suggest `permissions: {}` as the way to explicitly request that no permissions be given to the workflow so that's always what I've done.

(I couldn't find anywhere else in uv where `permissions:` is used with a missing block mapping though, so maybe I misunderstood you?)

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:43 on 2025-08-06 15:24_

Yep i meant empty without a trailing block, such as at: https://github.com/astral-sh/uv/blob/60ddaddc9cdbb1a4561acb87296dd15f62aaf208/.github/workflows/ci.yml#L6-L7

If it's the GitHub suggestion let's do that, this is a nit anyway.

---

_@konstin reviewed on 2025-08-06 15:24_

---

_Merged by @zanieb on 2025-08-06 15:52_

---

_Closed by @zanieb on 2025-08-06 15:52_

---

_Branch deleted on 2025-08-06 15:52_

---

_@woodruffw reviewed on 2025-08-06 16:00_

---

_Review comment by @woodruffw on `.github/workflows/build-binaries.yml`:43 on 2025-08-06 16:00_

Ah, gotcha! Yeah, AFAIK there's no semantic difference between those. 

---
