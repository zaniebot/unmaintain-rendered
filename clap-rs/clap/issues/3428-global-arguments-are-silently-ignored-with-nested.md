```yaml
number: 3428
title: Global arguments are silently ignored with nested subcommands
type: issue
state: closed
author: kuecks
labels:
  - C-bug
  - A-parsing
  - E-easy
assignees: []
created_at: 2022-02-09T21:07:50Z
updated_at: 2022-02-09T22:30:33Z
url: https://github.com/clap-rs/clap/issues/3428
synced_at: 2026-01-12T16:14:15Z
```

# Global arguments are silently ignored with nested subcommands

---

_@kuecks_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.0-nightly (65f3f8b22 2021-11-21)

### Clap Version

3.0.14

### Minimal reproducible code

```rust
use clap::{Parser, Args, Subcommand}; // 3.0.14

#[derive(Parser, Debug)]
struct Options {
    #[clap(subcommand)]
    command: Command1,
}

#[derive(Subcommand, Debug)]
enum Command1 {
    A(Command1AOptions),
}

#[derive(Args, Debug)]
struct Command1AOptions {
    #[clap(subcommand)]
    command: Command2,
    #[clap(long, global = true)]
    flag1: bool,
}

#[derive(Subcommand, Debug)]
enum Command2 {
    B {
        #[clap(subcommand)]
        command: Command3,
        #[clap(long, global = true)]
        flag2: bool,
    },
}

#[derive(Subcommand, Debug)]
enum Command3 {
    C(Command3COptions),
}

#[derive(Args, Debug)]
struct Command3COptions {}

fn main() {
    let options = Options::parse();
    dbg!(options);
}
```


### Steps to reproduce the bug with the above code

`cargo run -- a b c --flag2 --flag1`

This passes both global flags at the lowest level (flag1 is declared with `a`, flag2 is declared with `b`)

### Actual Behaviour

flag1 is correctly set to `true`, but flag2 is `false`!

```
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap_test a b c --flag2 --flag1`
[src/main.rs:42] options = Options {
    command: A(
        Command1AOptions {
            command: B {
                command: C(
                    Command3COptions,
                ),
                flag2: false,
            },
            flag1: true,
        },
    ),
}
```

### Expected Behaviour

Both `flag1` and `flag2` should be `true`

### Additional Context

_No response_

### Debug Output

[debug.txt](https://github.com/clap-rs/clap/files/8036228/debug.txt)


---

_Label `C-bug` added by @kuecks on 2022-02-09 21:07_

---

_Label `A-parsing` added by @epage on 2022-02-09 21:36_

---

_Label `E-easy` added by @epage on 2022-02-09 21:36_

---

_Comment by @epage on 2022-02-09 21:36_

I think I've identified root cause and am working on the fix.  Our recursive discovery of global flags appears to be broken,

---

_Referenced in [clap-rs/clap#3429](../../clap-rs/clap/pulls/3429.md) on 2022-02-09 21:54_

---

_Closed by @epage on 2022-02-09 22:23_

---

_Comment by @kuecks on 2022-02-09 22:26_

@epage Wow that was very fast! Thank you very much!

---

_Comment by @epage on 2022-02-09 22:30_

The fix was fast but the part that might be slow is the release.  I generally aim to release as soon as a user-facing change is in but I'm batching up some deprecations, so focusing on finish that first.  Sorry for the delay.

---
