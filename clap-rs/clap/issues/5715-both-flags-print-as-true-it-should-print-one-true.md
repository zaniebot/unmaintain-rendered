---
number: 5715
title: "Both Flags Print as `true`, it should print one `true` ant other `false`"
type: issue
state: closed
author: BiswajitThakur
labels:
  - C-bug
assignees: []
created_at: 2024-09-03T14:13:32Z
updated_at: 2024-09-03T14:42:41Z
url: https://github.com/clap-rs/clap/issues/5715
synced_at: 2026-01-10T01:28:15Z
---

# Both Flags Print as `true`, it should print one `true` ant other `false`

---

_Issue opened by @BiswajitThakur on 2024-09-03 14:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.1 (3f5fd8dd4 2024-08-06)

### Clap Version

clap = "4.5.16"

### Minimal reproducible code

1. `src/main.rs`

```rust
use clap::{self, Arg, ArgAction, Command};

fn main() {
    let app = Command::new(env!("CARGO_BIN_NAME"))
        .subcommand(
            Command::new("cmd")
                .long_flag("cmd")
                .arg(Arg::new("aa").long("aa").action(ArgAction::SetTrue))
                .arg(Arg::new("bb").long("bb").action(ArgAction::SetTrue)),
        )
        .get_matches();
    match app.subcommand() {
        Some(("cmd", arg)) => {
            println!("aa: {}", arg.contains_id("aa"));
            println!("bb: {}", arg.contains_id("bb"));
        }
        _ => unreachable!(),
    }
}
```

2. `Cargo.toml`

```toml
[package]
name = "clp"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = "4.5.16"
```

### Steps to reproduce the bug with the above code

```
cargo new clp
cd clp
cargo add clap
```



### Actual Behaviour

```
eagle@hp-lp:~/development/clp$ cargo run -- cmd --aa
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clp cmd --aa`
aa: true
bb: true
eagle@hp-lp:~/development/clp$ cargo run -- cmd --bb
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clp cmd --bb`
aa: true
bb: true
```

### Expected Behaviour

when I run `cargo run -- cmd --aa`, it should print following output.
```
aa: true
bb: false
```

but It print unexpected output
```
aa: true
bb: true
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @BiswajitThakur on 2024-09-03 14:13_

---

_Comment by @epage on 2024-09-03 14:42_

tl;dr use `get_flag` instead of `contains_id`.

> ```rust
>             println!("aa: {}", arg.contains_id("aa"));
>             println!("bb: {}", arg.contains_id("bb"));
> ```

Might not be fully obvious but from the [`contains_id` docs](https://docs.rs/clap/latest/clap/struct.ArgMatches.html#method.contains_id):

> NOTE: This will always return true if [default_value](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value) has been set. [ArgMatches::value_source](https://docs.rs/clap/latest/clap/struct.ArgMatches.html#method.value_source) can be used to check if a value is present at runtime.

If you look at the [`SetTrue` docs](https://docs.rs/clap/latest/clap/enum.ArgAction.html#variant.SetTrue), it calls out that a `default_value` is set automatically and the examples show that `contains_id` is always true while `get_flag` changes based on whether the flag is present or not.

---

_Closed by @epage on 2024-09-03 14:42_

---
