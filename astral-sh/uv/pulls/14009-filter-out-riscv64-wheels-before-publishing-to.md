```yaml
number: 14009
title: filter out riscv64 wheels before publishing to pypi
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/filterv64
created_at: 2025-06-12T22:31:44Z
updated_at: 2025-06-13T13:57:05Z
url: https://github.com/astral-sh/uv/pull/14009
synced_at: 2026-01-12T16:10:58Z
```

# filter out riscv64 wheels before publishing to pypi

---

_@Gankra_

An alternative to #14006 

---

_Label `internal` added by @Gankra on 2025-06-12 22:31_

---

_Comment by @Gankra on 2025-06-12 22:31_

cc @Xeonacid 

---

_Comment by @Xeonacid on 2025-06-12 22:38_

Thanks a lot, Gankra!

---

_@zanieb reviewed on 2025-06-12 22:42_

We should avoid building these wheels, right? 🤔 

---

_Comment by @Gankra on 2025-06-12 23:14_

> We should avoid building these wheels, right? 🤔

I believe pypi is the bottleneck here and the wheels are *technically* usable if you hand them to like, uv or whatever to install them. Although since we don't upload them to the github release it's "know they're attached to the release workflow as random artifacts".

---

_Assigned to @konstin by @zanieb on 2025-06-13 12:14_

---

_Comment by @konstin on 2025-06-13 12:35_

I have a prototype for a feature in maturin where it validates that only pypi-compatible wheels are build (`maturin build --compatibility pypi`), so in preparation for that, I'd be in favor of not building the wheels at all (the binaries can still go on the GitHub releases page).

---

_Comment by @Gankra on 2025-06-13 12:46_

Cool, i'll do a third crack at this

---

_@konstin approved on 2025-06-13 13:54_

---

_Comment by @Gankra on 2025-06-13 13:56_

Notes:
* The maturin action doesn't provide a way to build the binaries without the wheels
* Dropping maturin would make this platform more annoying to build for
* If we're building the wheels anyway we might as well yeet them into the artifacts for the CI run and use the wheel tests as smoke tests for the platform
* We can omit `compatibility: pypi` for this one build if maturin adds that feature

---

_Merged by @Gankra on 2025-06-13 13:57_

---

_Closed by @Gankra on 2025-06-13 13:57_

---

_Branch deleted on 2025-06-13 13:57_

---
