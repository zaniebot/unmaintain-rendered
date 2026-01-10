---
number: 2847
title: "Using `help_heading` in one place requires using it on all Flags to get a Unified experience"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2021-10-11T16:04:41Z
updated_at: 2021-10-13T22:45:13Z
url: https://github.com/clap-rs/clap/issues/2847
synced_at: 2026-01-10T01:27:27Z
---

# Using `help_heading` in one place requires using it on all Flags to get a Unified experience

---

_Issue opened by @epage on 2021-10-11 16:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

v3.0.0-beta.4

### Minimal reproducible code

```rust
fn main() {
    use clap::*;
    let app = App::new("myTest")
        .name("test")
        .author("Kevin K.")
        .about("tests stuff")
        .version("1.3")
        .arg(Arg::from("-f, --flag 'some flag'"))
        .arg(Arg::from("[arg] 'some pos arg'"))
        .arg(Arg::from("--option [opt] 'some option'"))
        .arg(Arg::from("-v, --verbose 'Extra info'").help_heading(Some("Debug")))
        .arg(Arg::from("-q, --quiet 'Less info'").help_heading(Some("Debug")));
    app.get_matches();
}
```
or
```rust
fn main() {
    use clap::*;
    let app = App::new("myTest")
        .name("test")
        .author("Kevin K.")
        .about("tests stuff")
        .version("1.3")
        .setting(AppSettings::UnifiedHelpMessage)
        .arg(Arg::from("-f, --flag 'some flag'"))
        .arg(Arg::from("[arg] 'some pos arg'"))
        .arg(Arg::from("--option [opt] 'some option'"))
        .arg(Arg::from("-v, --verbose 'Extra info'").help_heading(Some("Debug")))
        .arg(Arg::from("-q, --quiet 'Less info'").help_heading(Some("Debug")));
    app.get_matches();
}
```

### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

```
❯ cargo run -- --help
   Compiling test-clap v0.1.0 (/home/epage/src/personal/test-clap)
    Finished dev [unoptimized + debuginfo] target(s) in 0.60s
     Running `target/debug/test-clap --help`
test 1.3

Kevin K.

tests stuff

USAGE:
    test-clap [FLAGS] [OPTIONS] [arg]

ARGS:
    <arg>    some pos arg

FLAGS:
    -f, --flag       some flag
    -h, --help       Print help information
    -V, --version    Print version information

OPTIONS:
        --option <opt>    some option

Debug:
    -q, --quiet      Less info
    -v, --verbose    Extra info
```
or
```
❯ cargo run -- --help
   Compiling test-clap v0.1.0 (/home/epage/src/personal/test-clap)
    Finished dev [unoptimized + debuginfo] target(s) in 0.63s
     Running `target/debug/test-clap --help`
test 1.3

Kevin K.

tests stuff

USAGE:
    test-clap [OPTIONS] [arg]

ARGS:
    <arg>    some pos arg

OPTIONS:
    -f, --flag            some flag
    -h, --help            Print help information
        --option <opt>    some option
    -q, --quiet           Less info
    -v, --verbose         Extra info
    -V, --version         Print version information
```

### Expected Behaviour

Without having to override `help_heading` on every flag:
```
❯ cargo run -- --help
   Compiling test-clap v0.1.0 (/home/epage/src/personal/test-clap)
    Finished dev [unoptimized + debuginfo] target(s) in 0.60s
     Running `target/debug/test-clap --help`
test 1.3

Kevin K.

tests stuff

USAGE:
    test-clap [FLAGS] [OPTIONS] [arg]

ARGS:
    <arg>    some pos arg

OPTIONS:
    -f, --flag       some flag
    -h, --help       Print help information
    -V, --version    Print version information
        --option <opt>    some option

Debug:
    -q, --quiet      Less info
    -v, --verbose    Extra info
```

### Additional Context

This is split off of #2807

We could fix this by respecting `help_heading` with `UnifiedHelpMessage` but it then seems like a misnomer because it is no longer unified.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-11 16:04_

---

_Added to milestone `3.0` by @epage on 2021-10-11 16:04_

---

_Referenced in [clap-rs/clap#2807](../../clap-rs/clap/issues/2807.md) on 2021-10-11 16:18_

---

_Label `C: help message` added by @pksunkara on 2021-10-11 16:29_

---

_Label `C: usage strings` added by @pksunkara on 2021-10-11 16:29_

---

_Closed by @bors[bot] on 2021-10-13 22:45_

---
