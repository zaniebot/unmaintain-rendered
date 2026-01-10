```yaml
number: 8825
title: "Use `Ranges::from_iter` over `Ranges::iter_mut`"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: tracking/050
head: konsti/charlie/local
created_at: 2024-11-05T08:03:01Z
updated_at: 2024-11-06T09:27:44Z
url: https://github.com/astral-sh/uv/pull/8825
synced_at: 2026-01-10T12:00:00Z
```

# Use `Ranges::from_iter` over `Ranges::iter_mut`

---

_Pull request opened by @konstin on 2024-11-05 08:03_

Use https://github.com/astral-sh/pubgrub/pull/34 (https://github.com/pubgrub-rs/pubgrub/pull/273) for safer ranges modification that can't break pubgrub invariants.

In the process, I removed the `&mut` references in favor of a direct immutable-to-immutable mapping.

The tests currently fail, i think it's because we're producing empty segments where `Ranges` expects them to be non-empty; See https://github.com/pubgrub-rs/pubgrub/pull/273 for more discussion on invariants.

---

_Review requested from @charliermarsh by @konstin on 2024-11-05 08:03_

---

_Comment by @zanieb on 2024-11-05 13:22_

Should we change https://github.com/astral-sh/uv/blob/0335efd0a901ee2351e663d0488b1ce87fd4cc80/crates/pep508-rs/src/marker/algebra.rs#L638-L645 and https://github.com/astral-sh/uv/blob/e1a8beb64b9f6eb573b3af616ffff680c282a316/crates/uv-resolver/src/pubgrub/report.rs#L1177-L1180 too?

---

_Comment by @konstin on 2024-11-05 13:37_

Do those match the invariants from https://github.com/pubgrub-rs/pubgrub/pull/273, or do those need `Ranges:from_unsorted` from https://github.com/pubgrub-rs/pubgrub/pull/273?

---

_Comment by @zanieb on 2024-11-05 15:46_

I don't know! :)

---

_Closed by @konstin on 2024-11-06 09:27_

---
