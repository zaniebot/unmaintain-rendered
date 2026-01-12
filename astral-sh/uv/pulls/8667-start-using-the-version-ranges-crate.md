```yaml
number: 8667
title: " Start using the version ranges crate"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/version-ranges-1
created_at: 2024-10-29T16:31:51Z
updated_at: 2024-10-29T16:39:51Z
url: https://github.com/astral-sh/uv/pull/8667
synced_at: 2026-01-12T16:08:26Z
```

#  Start using the version ranges crate

---

_@konstin_

See https://github.com/pubgrub-rs/pubgrub/pull/262 for prior discussion.

In most parts of uv, we don't want to depend on pubgrub, but we want to use its powerful version ranges arithmetic. https://github.com/pubgrub-rs/pubgrub/pull/262 split out the ranges into a separate, small crate (that can be published separately to crates.io) that contains just the `Ranges` type.

With this first in a series of changes, we only depend on pubgrub in the uv-resolver crate. This enables us to directly depend on `Ranges` in the pep440 and pep508 crates without blocking downstream consumers (direct or through the crates.io versions), and also merging uv-pubgrub into the pep440 crate.

Note that the name is now `Ranges`, since the type formerly known as `Range` was a misnomer, it contains multiple ranges.


---

_Label `internal` added by @konstin on 2024-10-29 16:31_

---

_@zanieb approved on 2024-10-29 16:35_

---

_Merged by @konstin on 2024-10-29 16:39_

---

_Closed by @konstin on 2024-10-29 16:39_

---

_Branch deleted on 2024-10-29 16:39_

---
