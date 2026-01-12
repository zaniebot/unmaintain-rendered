```yaml
number: 1803
title: Improve debug implementation of Id
type: issue
state: closed
author: pksunkara
labels:
  - C-enhancement
assignees: []
created_at: 2020-04-10T07:25:02Z
updated_at: 2020-04-14T21:38:05Z
url: https://github.com/clap-rs/clap/issues/1803
synced_at: 2026-01-12T16:14:11Z
```

# Improve debug implementation of Id

---

_@pksunkara_

I have an idea: Id can be represented as
```rust
struct Id {
    #[cfg(debug_assertions)]
    name: String,
    id: u64
}
```

instead of `type Id = u64`, possibly along with `Deref<u64>`, `PartialEq` and so on. This would also allow for nicer `Debug` impls for `Arg & App`.

I'll work on it after I'm done with interpolation.

_Originally posted by @CreepySkeleton in https://github.com/_render_node/MDIzOlB1bGxSZXF1ZXN0UmV2aWV3VGhyZWFkMjUyMjU2ODA4OnYy/pull_request_review_threads/discussion_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-10 07:25_

---

_Label `C: asserts` added by @pksunkara on 2020-04-10 07:25_

---

_Label `D: medium` added by @pksunkara on 2020-04-10 07:25_

---

_Label `P1: urgent` added by @pksunkara on 2020-04-10 07:25_

---

_Label `T: enhancement` added by @pksunkara on 2020-04-10 07:25_

---

_Referenced in [clap-rs/clap#1809](../../clap-rs/clap/issues/1809.md) on 2020-04-12 00:12_

---

_Referenced in [clap-rs/clap#1821](../../clap-rs/clap/pulls/1821.md) on 2020-04-14 02:08_

---

_Closed by @bors[bot] on 2020-04-14 21:38_

---
