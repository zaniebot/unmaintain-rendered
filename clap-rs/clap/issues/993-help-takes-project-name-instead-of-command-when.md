```yaml
number: 993
title: help takes project name instead of command when printing help for sub command
type: issue
state: closed
author: sagiegurari
labels: []
assignees: []
created_at: 2017-07-04T03:46:33Z
updated_at: 2018-08-02T03:30:08Z
url: https://github.com/clap-rs/clap/issues/993
synced_at: 2026-01-12T16:14:10Z
```

# help takes project name instead of command when printing help for sub command

---

_@sagiegurari_

### Rust Version

rustc 1.20.0-nightly (05b579766 2017-07-01)
binary: rustc
commit-hash: 05b5797664d6aeaa0c7d0606610f336fe0b57e97
commit-date: 2017-07-01
host: x86_64-unknown-linux-gnu
release: 1.20.0-nightly
LLVM version: 4.0

### Affected Version of clap

[dependencies]
clap = "^2.25.0"

### Expected Behavior Summary

cargo subcommand -h will print in both name and usage

command subcommand ...

### Actual Behavior Summary

in name
projectname-subcommand

in usage
projectname subcommand ....

### Steps to Reproduce the issue

use the below code and run

````sh
cargo run -- something -h
````

### Sample Code or Link to Sample Code

````rust
extern crate clap;
use clap::{App, SubCommand};

fn main() {
    App::new("cargo").subcommand(SubCommand::with_name("something").version("1").about("test clap")).get_matches();
}
````

### Debug output

see file for full debug info
[debug.txt](https://github.com/kbknapp/clap-rs/files/1120943/debug.txt)

at the end see:

````console
misc-test-something 1
test clap

USAGE:
    misc-test something

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
````


---

_Comment by @Arnavion on 2017-08-14 23:47_

That is by design. It uses the binary name set via `App::bin_name()` which defaults to the crate name.

---

_Comment by @sagiegurari on 2017-08-15 07:07_

So why 'cratename-subcommand' and not 'cratename subcommand'?
Anyway, the bin_name was a good clue and I see I can modify that, so i'll close the issue and just set it manually.
thanks

https://docs.rs/clap/2.26.0/clap/struct.App.html#method.bin_name

---

_Closed by @sagiegurari on 2017-08-15 07:07_

---

_Comment by @Arnavion on 2017-08-15 07:58_

It's a convention that if you want an application `foo` to use a subcommand `bar`, the implementation of the subcommand is a file named `foo-bar`. For example that's why external cargo subcommands like clippy have a binary named `cargo-clippy` (hyphen) that you invoke using `cargo clippy` (space).

Even when the subcommand is not actually a separate binary, the convention is still used in its help. Eg: https://git-scm.com/docs/git-log

---
