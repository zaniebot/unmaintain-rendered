```yaml
number: 5926
title: "feat(builder): Add `ValueParserFactory` for `Saturating<T>`"
type: pull_request
state: merged
author: sorairolake
labels: []
assignees: []
merged: true
base: master
head: feature/value-parser-factory-for-saturating
created_at: 2025-02-24T02:48:29Z
updated_at: 2025-02-24T22:25:50Z
url: https://github.com/clap-rs/clap/pull/5926
synced_at: 2026-01-12T16:14:17Z
```

# feat(builder): Add `ValueParserFactory` for `Saturating<T>`

---

_@sorairolake_

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->

The MSRV of this project is 1.74, and [`Saturating`](https://doc.rust-lang.org/std/num/struct.Saturating.html) was stabilized in v1.74.0, so this can be implemented. Note that `ValueParserFactory` for [`Wrapping`](https://doc.rust-lang.org/std/num/struct.Wrapping.html) is already implemented.

---
