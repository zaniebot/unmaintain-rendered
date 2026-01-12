```yaml
number: 2749
title: "cargo: nuke 'simd-accel' from orbit"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/nuke-simd-accel
created_at: 2024-03-07T14:40:24Z
updated_at: 2024-03-07T14:47:46Z
url: https://github.com/BurntSushi/ripgrep/pull/2749
synced_at: 2026-01-12T18:23:14Z
```

# cargo: nuke 'simd-accel' from orbit

---

_@BurntSushi_

This feature causes nothing but problems and is frequently broken. The
only optimization it was enabling were SIMD optimizations for
transcoding. In particular, for UTF-16 transcoding. This is performed by
the [`encoding_rs`](https://github.com/hsivonen/encoding_rs) crate,
which specifically uses unstable portable SIMD APIs instead of the
stable non-portable SIMD APIs.

SIMD optimizations that apply to search have long been making use of
stable APIs, and are automatically enabled when your target supports
them. This is, IMO, the correct user experience and one that
`encoding_rs` refuses to support. I'm done dealing with it, so
transcoding will only use scalar code until the SIMD optimizations in
`encoding_rs` work on stable. (This doesn't mean that `encoding_rs` has
to change. This could also be fixed by stabilizing `std::simd`.)

Fixes #2748


---

_Merged by @BurntSushi on 2024-03-07 14:47_

---

_Closed by @BurntSushi on 2024-03-07 14:47_

---

_Branch deleted on 2024-03-07 14:47_

---
