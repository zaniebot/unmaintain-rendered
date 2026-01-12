```yaml
number: 2378
title: "Try harder to fix #916"
type: pull_request
state: closed
author: tavianator
labels: []
assignees: []
base: master
head: break-after-errors
created_at: 2022-12-28T18:13:25Z
updated_at: 2023-07-07T18:47:56Z
url: https://github.com/BurntSushi/ripgrep/pull/2378
synced_at: 2026-01-12T18:23:14Z
```

# Try harder to fix #916

---

_@tavianator_

The underlying Rust issue[1] was recently re-introduced and re-fixed.
Rather than wait for a new Rust release, apply effectively the same fix
as a workaround in the ignore crate itself.

[1]: https://github.com/rust-lang/rust/issues/50619


---

_@tmccombs approved on 2022-12-28 18:36_

---

_Comment by @BurntSushi on 2023-07-07 18:47_

My PR review latency is very high, so this sort of strategy tends not to work so well unless we happen to get lucky and I see and respond to it quickly. But in this case, it looks like a few Rust releases have come out since the issue was re-fixed, so I think we can avoid bringing this in.

---

_Closed by @BurntSushi on 2023-07-07 18:47_

---
