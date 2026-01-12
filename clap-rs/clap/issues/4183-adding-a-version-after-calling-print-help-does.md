```yaml
number: 4183
title: "Adding a version after calling `print_help` does not update for another `print_help`"
type: issue
state: closed
author: ModProg
labels:
  - C-bug
assignees: []
created_at: 2022-09-06T10:09:24Z
updated_at: 2022-09-06T14:08:06Z
url: https://github.com/clap-rs/clap/issues/4183
synced_at: 2026-01-12T16:14:15Z
```

# Adding a version after calling `print_help` does not update for another `print_help`

---

_@ModProg_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::Command;

fn main() {
    let mut command = Command::new("test");
    command.print_help();
    command = command.version("123");
    command.print_help();
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

prints same help twice:
```
Usage:
    test

Options:
    -h, --help    Print help information
Usage:
    test

Options:
    -h, --help    Print help information
```

### Expected Behaviour

prints `-V` on second help:
```
Usage:
    test

Options:
    -h, --help    Print help information
Usage:
    test

Options:
    -h, --help       Print help information
    -V, --version    Print version information
```

### Additional Context

also applies to parsing i.e. `-V` will show an error that it does not exists.

### Debug Output

_No response_

---

_Label `C-bug` added by @ModProg on 2022-09-06 10:09_

---

_Comment by @epage on 2022-09-06 14:08_

I believe this would be a subset of https://github.com/clap-rs/clap/issues/2911

---

_Closed by @epage on 2022-09-06 14:08_

---
