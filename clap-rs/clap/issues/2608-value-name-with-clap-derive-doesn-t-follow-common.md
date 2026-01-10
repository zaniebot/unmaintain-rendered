---
number: 2608
title: "value name with clap_derive doesn't follow common practices"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-07-19T21:37:05Z
updated_at: 2021-07-27T08:06:39Z
url: https://github.com/clap-rs/clap/issues/2608
synced_at: 2026-01-10T01:27:21Z
---

# value name with clap_derive doesn't follow common practices

---

_Issue opened by @epage on 2021-07-19 21:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
#[derive(Clap)]
struct Args {
    #[clap(long)]
    value: String,
}
```


### Steps to reproduce the bug with the above code

Run that with `--help`

### Actual Behaviour

Help shows `--value value`

### Expected Behaviour

Help shows `--value VALUE`

### Additional Context

Clap itself says to do this

> Although not required, it's somewhat convention to use all capital letters for the value name.

From https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.value_name

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-07-19 21:37_

---

_Referenced in [clap-rs/clap#2611](../../clap-rs/clap/pulls/2611.md) on 2021-07-21 16:59_

---

_Label `C: derive macros` added by @pksunkara on 2021-07-25 13:35_

---

_Closed by @pksunkara on 2021-07-27 08:06_

---
