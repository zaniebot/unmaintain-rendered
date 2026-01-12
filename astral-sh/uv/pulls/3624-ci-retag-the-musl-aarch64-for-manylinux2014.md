```yaml
number: 3624
title: "ci: retag the musl aarch64 for manylinux2014"
type: pull_request
state: merged
author: henryiii
labels:
  - releases
assignees: []
merged: true
base: main
head: henryiii/ci/muslarm
created_at: 2024-05-16T06:51:45Z
updated_at: 2024-05-18T17:51:10Z
url: https://github.com/astral-sh/uv/pull/3624
synced_at: 2026-01-12T16:05:45Z
```

# ci: retag the musl aarch64 for manylinux2014

---

_@henryiii_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix #3439 by retagging the statically linked musl binary for manylinux2014. This is the most conservative option;
the regular manylinux one could be removed as well, and I expect other archs could probably do this as well.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Testing on manylinux.

<!-- How was it tested? -->


---

_Renamed from "ci: retag the musl arm for manylinux2014" to "ci: retag the musl aarch64 for manylinux2014" by @henryiii on 2024-05-16 07:26_

---

_@konstin requested changes on 2024-05-16 07:37_

Can we do this with `maturin build --target x86_64-unknown-linux-musl --compatibility 2_17 musllinux_1_1 [...]` in the maturin action step? This way maturin will enforce {many,musl}linux compliance

---

_Label `release` added by @konstin on 2024-05-16 07:37_

---

_@konstin approved on 2024-05-16 14:58_

Thanks!

---

_Comment by @henryiii on 2024-05-16 15:07_

Should the original higher manylinux one be kept? Should this be done on other archs?

---

_Assigned to @konstin by @zanieb on 2024-05-17 01:59_

---

_Comment by @konstin on 2024-05-17 08:54_

For this PR, i'd keep the change small an only change this build. But in general: Yes please! A single manylinux + musllinux tagged build per arch would be great

---

_Merged by @konstin on 2024-05-17 08:54_

---

_Closed by @konstin on 2024-05-17 08:54_

---

_Branch deleted on 2024-05-18 17:51_

---
