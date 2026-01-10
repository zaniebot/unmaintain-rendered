---
number: 3709
title: No way to rename value names across entire struct
type: issue
state: open
author: epage
labels:
  - C-bug
  - M-breaking-change
  - E-medium
  - A-derive
assignees: []
created_at: 2022-05-09T15:43:40Z
updated_at: 2022-06-01T21:13:10Z
url: https://github.com/clap-rs/clap/issues/3709
synced_at: 2026-01-10T01:27:45Z
---

# No way to rename value names across entire struct

---

_Issue opened by @epage on 2022-05-09 15:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.1.13

### Minimal reproducible code

```rust
#[derive(Parser)]
struct Args {
    hello_world: String
}
```


### Steps to reproduce the bug with the above code

Run the `--help` for the above

### Actual Behaviour

Its listed as HELLO_WORLD

### Expected Behaviour

A way to rename it to hello-world

### Additional Context

This is split out of #3282.

Proposed solution
- Rename `rename_all_env` to `rename_env`
- Add `rename_value_name`
- Add `rename_long`
- Add `rename_command`
- Remove `rename_all`

Nothing will rename `id` or `short`.

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-05-09 15:43_

---

_Label `M-breaking-change` added by @epage on 2022-05-09 15:43_

---

_Label `E-medium` added by @epage on 2022-05-09 15:43_

---

_Label `A-derive` added by @epage on 2022-05-09 15:43_

---

_Added to milestone `4.0` by @epage on 2022-05-09 15:43_

---

_Removed from milestone `4.0` by @epage on 2022-06-01 21:13_

---

_Added to milestone `5.0` by @epage on 2022-06-01 21:13_

---

_Referenced in [clap-rs/clap#2475](../../clap-rs/clap/issues/2475.md) on 2022-06-08 16:06_

---

_Referenced in [clap-rs/clap#3792](../../clap-rs/clap/issues/3792.md) on 2022-06-09 14:06_

---

_Referenced in [clap-rs/clap#3785](../../clap-rs/clap/issues/3785.md) on 2022-08-03 13:11_

---

_Referenced in [clap-rs/clap#4869](../../clap-rs/clap/issues/4869.md) on 2023-04-28 20:42_

---
