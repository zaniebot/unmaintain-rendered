```yaml
number: 2363
title: "Remove `Range::bounds` usage"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-range-bounds-call
created_at: 2024-03-11T20:57:35Z
updated_at: 2024-03-11T21:10:17Z
url: https://github.com/astral-sh/uv/pull/2363
synced_at: 2026-01-12T16:04:59Z
```

# Remove `Range::bounds` usage

---

_@konstin_

I'm trying to reduce our pubgrub upstream divergences and since we only have one usage of our custom `Range::bounds` it seems more reasonable to do this in uv directly than in pubgrub (https://github.com/pubgrub-rs/pubgrub/pull/188#issuecomment-1989410636).

---

_Label `internal` added by @konstin on 2024-03-11 20:57_

---

_Review requested from @charliermarsh by @konstin on 2024-03-11 20:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:362 on 2024-03-11 21:01_

Nit: can we use `version` or `segment` instead of `v`?

---

_@charliermarsh approved on 2024-03-11 21:01_

---

_@konstin reviewed on 2024-03-11 21:04_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/report.rs`:362 on 2024-03-11 21:04_

good call

---

_Merged by @konstin on 2024-03-11 21:10_

---

_Closed by @konstin on 2024-03-11 21:10_

---

_Branch deleted on 2024-03-11 21:10_

---
