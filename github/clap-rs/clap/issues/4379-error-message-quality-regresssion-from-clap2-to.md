---
number: 4379
title: Error message quality regresssion from clap2 to clap3/4
type: issue
state: closed
author: matklad
labels:
  - C-bug
assignees: []
created_at: 2022-10-13T11:38:02Z
updated_at: 2022-10-13T12:12:19Z
url: https://github.com/clap-rs/clap/issues/4379
synced_at: 2026-01-07T13:12:20-06:00
---

# Error message quality regresssion from clap2 to clap3/4

---

_Issue opened by @matklad on 2022-10-13 11:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0-beta.6 (25912c097 2022-09-09)

### Clap Version

3.*.*, 4.*.*

### Minimal reproducible code

```rust
// clap 2
use clap::{App, Arg, SubCommand};
fn main() {
    let matches = App::new("app")
        .arg(
            Arg::with_name("config")
                .short("c")
                .long("config")
                .value_name("FILE")
                .help("Sets a custom config file")
                .takes_value(true),
        )
        .subcommand(SubCommand::with_name("sub"))
        .get_matches();
    eprintln!("matches = {:?}", matches);
}

// clap 3
use clap::{App, Arg, SubCommand};
fn main() {
    let matches = App::new("app")
        .arg(
            Arg::with_name("config")
                .short('c')
                .long("config")
                .value_name("FILE")
                .help("Sets a custom config file")
                .takes_value(true),
        )
        .subcommand(SubCommand::with_name("sub"))
        .get_matches();
    eprintln!("matches = {:?}", matches);
}


// clap 4
use clap::{builder::{Arg, Command}, ArgAction};
fn main() {
    let matches = Command::new("app")
        .arg(
            Arg::new("config")
                .short('c')
                .long("config")
                .value_name("FILE")
                .help("Sets a custom config file")
                .action(ArgAction::Set),
        )
        .subcommand(Command::new("sub"))
        .get_matches();
    eprintln!("matches = {:?}", matches);
}


```

### Steps to reproduce the bug with the above code

Run the above three binaries with `sub -c foo` args.

Clap2 gives correct, if not maximally helpful, error message: 

```
error: Found argument '-c' which wasn't expected, or isn't valid in this context

USAGE:
    clap-repro sub
```

Clap3 and clap4 give actively misleading error messages: 

```
error: Found argument '-c' which wasn't expected, or isn't valid in this context

	If you tried to supply `-c` as a value rather than a flag, use `-- -c`

USAGE:
    clap-repro sub
```

```
error: Found argument '-c' which wasn't expected, or isn't valid in this context

  If you tried to supply '-c' as a value rather than a flag, use '-- -c'

Usage: clap-repro sub
```

### Actual Behaviour

-

### Expected Behaviour

-

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @matklad on 2022-10-13 11:38_

---

_Referenced in [near/nearcore#7732](../../near/nearcore/pulls/7732.md) on 2022-10-13 11:40_

---

_Comment by @epage on 2022-10-13 12:12_

This looks to be a duplicate of #2766.  If I misunderstood, let me know and I can re-open this.

In #2766 could you clarify what you are not liking and why it caused confusion about the CLI in https://github.com/near/nearcore/pull/7732?

---

_Closed by @epage on 2022-10-13 12:12_

---
