```yaml
number: 2448
title: "Singleton optional values should use `[<name>]` in help message"
type: issue
state: closed
author: mina86
labels:
  - A-help
  - A-derive
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-04-20T20:59:56Z
updated_at: 2021-08-02T08:19:20Z
url: https://github.com/clap-rs/clap/issues/2448
synced_at: 2026-01-12T16:14:13Z
```

# Singleton optional values should use `[<name>]` in help message

---

_@mina86_

### Rust Version

rustc 1.53.0-nightly (9d9c2c92b 2021-04-19)

### Affected Version of clap

3.0.0-beta.2

### Expected Behavior Summary

Help message for `Option<Option<type>>` switches uses `[<name>]` rather than `<name>...` to indicate that the value is optional.  For example, for switch `opt` the help screen reads `--opt [<opt>]`.  Ellipsis suggests that the value can be provided more than once (which is not correct) whereas square brackets correctly indicate the value can be given at most once.  

### Actual Behavior Summary

Ellipsis is used in help message.  For switch `opt` the help screen reads `--opt <opt>...`.

### Steps to Reproduce the issue

```
use clap::Clap;

#[derive(Clap, Debug)]
struct Opts {
    #[clap(long)]
    opt: Option<Option<i32>>
}

fn main() {
    println!("{:?}", Opts::parse());
}
```

```
$ cargo --quiet run -- --help |grep -e --opt
        --opt <opt>...
# Expected:         --opt [<opt>]
```

---

_Comment by @pksunkara on 2021-04-25 15:55_

Can you please add more context on why `Option<Option<i32>>` is used instead of `Option<i32>`?

---

_Added to milestone `3.1` by @pksunkara on 2021-04-25 15:55_

---

_Label `C: derive macros` added by @pksunkara on 2021-04-25 15:55_

---

_Label `C: help message` added by @pksunkara on 2021-04-25 15:55_

---

_Comment by @mina86 on 2021-04-25 20:01_

`Option<i32>` makes the argument of the switch required which is something I don’t want.  I want `--opt` to enable particular feature while allow `--opt=<value>` to customise an argument of that feature. For example, it could be `--parallel=[<count>]` switch which enables parallel processing defaulting to number of jobs equal number of CPUs.

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-06-16 03:30_

---

_Referenced in [clap-rs/clap#2653](../../clap-rs/clap/pulls/2653.md) on 2021-08-01 22:16_

---

_Closed by @pksunkara on 2021-08-02 08:19_

---
