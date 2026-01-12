```yaml
number: 3504
title: methods in attributes are not allowed for subcommand
type: issue
state: closed
author: epage
labels:
  - C-bug
  - E-medium
  - A-derive
assignees: []
created_at: 2022-02-23T15:23:19Z
updated_at: 2022-02-23T15:36:55Z
url: https://github.com/clap-rs/clap/issues/3504
synced_at: 2026-01-12T16:14:15Z
```

# methods in attributes are not allowed for subcommand

---

_@epage_

### Discussed in https://github.com/clap-rs/clap/discussions/3497

<div type='discussions-op-text'>

<sup>Originally posted by **messense** February 22, 2022</sup>
I want to reuse a subcommand from another crate A in crate B, but I don't want it to show in `--help`, adding `#[clap(subcommand, hide = true)]` leads to compilation error: `methods in attributes are not allowed for subcommand`.

It seems that the only way to do it now is to set `hide = true` on that subcommand:

```rust
#[derive(Subcommand)]
#[clap(hide = true)]
enum {
    ...
}
```

But that means it will be hidden everywhere even in crate A while I only want to hide it in crate B.</div>

---

_Label `A-derive` added by @epage on 2022-02-23 15:23_

---

_Label `C-bug` added by @epage on 2022-02-23 15:23_

---

_Label `E-medium` added by @epage on 2022-02-23 15:23_

---

_Referenced in [clap-rs/clap#3505](../../clap-rs/clap/pulls/3505.md) on 2022-02-23 15:23_

---

_Closed by @epage on 2022-02-23 15:36_

---
