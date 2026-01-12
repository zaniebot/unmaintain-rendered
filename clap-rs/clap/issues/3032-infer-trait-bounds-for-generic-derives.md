```yaml
number: 3032
title: Infer trait bounds for generic derives
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - E-medium
  - A-derive
assignees: []
created_at: 2021-11-16T15:07:46Z
updated_at: 2021-12-13T22:17:22Z
url: https://github.com/clap-rs/clap/issues/3032
synced_at: 2026-01-12T16:14:14Z
```

# Infer trait bounds for generic derives

---

_@epage_

Building off of #2769 / #3023, we should allow inferring trait bounds.

So instead of 
```rust
#[derive(Parser)]
struct Opt<T: Args> {
    #[clap(flatten)]
    inner: T,
}
```
The user does
```rust
#[derive(Parser)]
struct Opt<T> {
    #[clap(flatten)]
    inner: T,
}
```
like they do with when deriving `Copy`, `Clone`, serde types, etc.

See https://serde.rs/attr-bound.html for some inspiration on issues to deal with.

---

_Label `T: enhancement` added by @epage on 2021-11-16 15:07_

---

_Label `C: derive macros` added by @epage on 2021-11-16 15:07_

---

_Referenced in [clap-rs/clap#3023](../../clap-rs/clap/pulls/3023.md) on 2021-11-16 15:08_

---

_Referenced in [epage/clapng#243](../../epage/clapng/issues/243.md) on 2021-12-06 23:13_

---

_Label `E-medium` added by @epage on 2021-12-13 22:17_

---

_Referenced in [cucumber-rs/cucumber#257](../../cucumber-rs/cucumber/issues/257.md) on 2023-02-16 12:51_

---
