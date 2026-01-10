---
number: 4232
title: Subcommand flags are separated by spaces while argument flags use commas
type: issue
state: closed
author: Trivernis
labels:
  - C-bug
assignees: []
created_at: 2022-09-19T08:14:08Z
updated_at: 2022-09-19T14:22:25Z
url: https://github.com/clap-rs/clap/issues/4232
synced_at: 2026-01-10T01:27:52Z
---

# Subcommand flags are separated by spaces while argument flags use commas

---

_Issue opened by @Trivernis on 2022-09-19 08:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.63.0

### Clap Version

3.2.22

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
#[clap()]
struct Args {
    #[clap(short)]
    arg: String,

    #[clap(subcommand)]
    subcommand: Command,
}

#[derive(Subcommand)]
enum Command {
    #[clap(name = "test", short_flag = 't', long_flag = "test")]
    Test,
}

fn main() {
    let _args = Args::parse();
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

The help output is
```
clap-test 

USAGE:
    clap-test -a <ARG> <SUBCOMMAND>

OPTIONS:
    -a <ARG>        
    -h, --help      Print help information

SUBCOMMANDS:
    help              Print this message or the help of the given subcommand(s)
    test -t --test    
```
Which separates the options-flags by comma while the subcommand flags are just separated by space. This behaviour is inconsistent. I personally would prefer to use comma for separation in both cases as it is easier to understand.

### Expected Behaviour

The help output should look like this:
```rust
clap-test 

USAGE:
    clap-test -a <ARG> <SUBCOMMAND>

OPTIONS:
    -a <ARG>        
    -h, --help      Print help information

SUBCOMMANDS:
    help              Print this message or the help of the given subcommand(s)
    test, -t, --test    
```

### Additional Context

This issue relates to https://github.com/crystal-linux/amethyst/issues/64

### Debug Output

_No response_

---

_Label `C-bug` added by @Trivernis on 2022-09-19 08:14_

---

_Referenced in [clap-rs/clap#4235](../../clap-rs/clap/pulls/4235.md) on 2022-09-19 14:08_

---

_Closed by @epage on 2022-09-19 14:22_

---

_Comment by @epage on 2022-09-19 14:22_

This was fixed in `master` which corresponds with the upcoming v4.0.0

---

_Referenced in [crystal-linux/amethyst#64](../../crystal-linux/amethyst/issues/64.md) on 2022-09-19 14:35_

---
