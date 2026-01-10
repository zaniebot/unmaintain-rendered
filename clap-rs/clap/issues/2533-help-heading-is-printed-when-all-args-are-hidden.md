---
number: 2533
title: Help heading is printed when all args are hidden
type: issue
state: closed
author: stevenengler
labels:
  - C-bug
assignees: []
created_at: 2021-06-12T03:25:55Z
updated_at: 2021-08-12T20:32:07Z
url: https://github.com/clap-rs/clap/issues/2533
synced_at: 2026-01-10T01:27:20Z
---

# Help heading is printed when all args are hidden

---

_Issue opened by @stevenengler on 2021-06-12 03:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.0 (88f19c6da 2021-05-03)

### Clap Version

master (585a7c955)

### Minimal reproducible code

```rust
use clap::{App, Arg};

fn main() {
    let mut app = App::new("blorp")
        .author("Will M.")
        .about("does stuff")
        .version("1.4")
        .arg(
            Arg::from("-f, --fake <some> <val> 'some help'")
        )
        .help_heading("NETWORKING")
        .arg(
            Arg::new("no-proxy")
                .about("Do not use system proxy settings")
                .hidden_short_help(true),
        )
        .help_heading("SPECIAL")
        .arg(
            Arg::from("-b, --birthday-song <song> 'Change which song is played for birthdays'")
        )
        .stop_custom_headings()
        .arg(
            Arg::new("server-addr")
                .about("Set server address")
                .help_heading(Some("NETWORKING"))
                .hidden_short_help(true),
        );
    app.write_help(&mut std::io::stdout()).unwrap();
}
```

### Steps to reproduce the bug with the above code

```bash
cargo run
```

### Actual Behaviour

```text
blorp 1.4

Will M.

does stuff

USAGE:
    blorp --fake <some> <val> --birthday-song <song> [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -f, --fake <some> <val>    some help

NETWORKING:


SPECIAL:
    -b, --birthday-song <song>    Change which song is played for birthdays
```

### Expected Behaviour

```text
blorp 1.4

Will M.

does stuff

USAGE:
    blorp --fake <some> <val> --birthday-song <song> [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -f, --fake <some> <val>    some help

SPECIAL:
    -b, --birthday-song <song>    Change which song is played for birthdays
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @stevenengler on 2021-06-12 03:25_

---

_Referenced in [clap-rs/clap#2534](../../clap-rs/clap/pulls/2534.md) on 2021-06-12 03:27_

---

_Closed by @pksunkara on 2021-08-12 20:32_

---
