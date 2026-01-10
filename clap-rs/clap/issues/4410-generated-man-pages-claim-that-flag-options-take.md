---
number: 4410
title: Generated man pages claim that flag options take values
type: issue
state: closed
author: bgilbert
labels:
  - C-bug
  - E-easy
  - A-man
assignees: []
created_at: 2022-10-20T19:55:01Z
updated_at: 2022-10-31T19:29:24Z
url: https://github.com/clap-rs/clap/issues/4410
synced_at: 2026-01-10T01:27:54Z
---

# Generated man pages claim that flag options take values

---

_Issue opened by @bgilbert on 2022-10-20 19:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (Fedora 1.64.0-1.fc36)

### Clap Version

4.0.17, clap_mangen 0.2.3

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser};

#[derive(Parser)]
/// Command to do things
pub struct Cmd {
    /// Do thing
    #[clap(long)]
    thing: bool,
}

fn main() {
    Cmd::parse();
    clap_mangen::Man::new(Cmd::command())
        .render(&mut std::io::stdout())
        .unwrap();
}
```

Cargo.toml:

```toml
[package]
name = "clap-man"
version = "0.0.0"
edition = "2021"

[dependencies]
clap = { version = "4", features = ["derive"] }
clap_mangen = "0.2"
```

### Steps to reproduce the bug with the above code

```
cargo run | man -l -
```

### Actual Behaviour

The option description for `--thing` claims that it takes an argument and that that argument defaults to false:

```
clap-man(1)                 General Commands Manual                clap-man(1)

NAME
       clap-man - Command to do things

SYNOPSIS
       clap-man [--thing] [-h|--help]

DESCRIPTION
       Command to do things

OPTIONS
       --thing=THING [default: false]
              Do thing

       -h, --help
              Print help information

                                   clap-man                        clap-man(1)
```

Of course, it doesn't:

```console
$ cargo run -- --thing=true
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-man --thing=true`
error: The value 'true' was provided to '--thing' but it wasn't expecting any more values

Usage: clap-man --thing

For more information try '--help'
```

### Expected Behaviour

The man page lists the option without any argument, as it did in clap 3.2 / clap_mangen 0.1:

```
clap-man(1)                 General Commands Manual                clap-man(1)

NAME
       clap-man - Command to do things

SYNOPSIS
       clap-man [--thing] [-h|--help]

DESCRIPTION
       Command to do things

OPTIONS
       --thing
              Do thing

       -h, --help
              Print help information

                                   clap-man                        clap-man(1)
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @bgilbert on 2022-10-20 19:55_

---

_Referenced in [coreos/coreos-installer#1021](../../coreos/coreos-installer/pulls/1021.md) on 2022-10-21 05:18_

---

_Label `E-easy` added by @epage on 2022-10-21 11:49_

---

_Label `A-man` added by @epage on 2022-10-21 11:49_

---

_Referenced in [clap-rs/clap#4432](../../clap-rs/clap/pulls/4432.md) on 2022-10-30 17:59_

---

_Closed by @epage on 2022-10-31 11:51_

---

_Comment by @epage on 2022-10-31 11:54_

Thanks!  This is now available in v0.2.4

---

_Comment by @bgilbert on 2022-10-31 19:29_

Looks like it's half-fixed.  I now get:

```
clap-man(1)                 General Commands Manual                clap-man(1)

NAME
       clap-man - Command to do things

SYNOPSIS
       clap-man [--thing] [-h|--help]

DESCRIPTION
       Command to do things

OPTIONS
       --thing=THING
              Do thing

       -h, --help
              Print help information

                                   clap-man                        clap-man(1)
```

`[default: false]` is gone but `--thing=THING` is still there, and the parser doesn't actually accept that syntax.

---

_Referenced in [clap-rs/clap#4443](../../clap-rs/clap/issues/4443.md) on 2022-11-02 18:37_

---
