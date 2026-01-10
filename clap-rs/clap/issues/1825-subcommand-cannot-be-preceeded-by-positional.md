---
number: 1825
title: Subcommand cannot be preceeded by positional argument if its value is a prefix of a subcommand
type: issue
state: closed
author: CreepySkeleton
labels:
  - C-bug
  - E-hard
  - E-help-wanted
assignees: []
created_at: 2020-04-14T23:11:06Z
updated_at: 2020-04-15T06:17:31Z
url: https://github.com/clap-rs/clap/issues/1825
synced_at: 2026-01-10T01:27:07Z
---

# Subcommand cannot be preceeded by positional argument if its value is a prefix of a subcommand

---

_Issue opened by @CreepySkeleton on 2020-04-14 23:11_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code
[Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=fcbfba45a027cf74524f5a1480f5eac4)

```rust
use clap::{App, Arg, SubCommand};

fn main() {
    let m = App::new("app")
        .arg(Arg::with_name("name"))
        .subcommand(SubCommand::with_name("apple"))
        .subcommand(SubCommand::with_name("banana"))
        .get_matches_from(&["test", "ap", "apple"]);
        
    println!("{:#?}", m);
}
```

### Steps to reproduce the issue

Run `cargo run -- ap` (or `ba`, or any other prefix)

### Version

* **Rust**: 1.42
* **Clap**: 2.33

### Actual Behavior Summary

```
error: The subcommand 'ap' wasn't recognized
	Did you mean 'apple'?

If you believe you received this message in error, try re-running with 'test -- ap'

USAGE:
    test [name] [SUBCOMMAND]

For more information try --help
```

### Expected Behavior Summary

`ap` must be treated as positional argument, not a subcommand

### Additional context

Originally reported in https://github.com/TeXitoi/structopt/issues/378


---

_Label `T: bug` added by @CreepySkeleton on 2020-04-14 23:11_

---

_Label `C: args` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `C: positional args` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `D: hard` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `help wanted` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `P3: want to have` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `T: refactor` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-04-14 23:12_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-04-14 23:12_

---

_Renamed from "Subcommand cannot be preceeded by optional positional argument if its value is a prefix of a subcommand" to "Subcommand cannot be preceeded by positional argument if its value is a prefix of a subcommand" by @CreepySkeleton on 2020-04-14 23:31_

---

_Comment by @pksunkara on 2020-04-15 06:17_

Duplicate of https://github.com/clap-rs/clap/issues/878.

---

_Closed by @pksunkara on 2020-04-15 06:17_

---

_Label `R: duplicate` added by @pksunkara on 2020-04-15 06:17_

---

_Removed from milestone `3.1` by @pksunkara on 2020-04-15 06:17_

---
