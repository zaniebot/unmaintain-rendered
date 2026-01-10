---
number: 1722
title: "`-- subcommand` does not work and clap suggests to use `-- subcommand` more."
type: issue
state: closed
author: nagisa
labels:
  - C-bug
assignees: []
created_at: 2020-03-04T16:53:49Z
updated_at: 2020-03-05T10:38:04Z
url: https://github.com/clap-rs/clap/issues/1722
synced_at: 2026-01-10T01:27:04Z
---

# `-- subcommand` does not work and clap suggests to use `-- subcommand` more.

---

_Issue opened by @nagisa on 2020-03-04 16:53_

# Rust Version

rustc 1.43.0-nightly (7760cd0fb 2020-02-19)

### Code

```rust
use clap::*;

fn main() {
    let matches = App::new("My Super Program")
                      .subcommand(SubCommand::with_name("test"))
                      .arg(Arg::with_name("config")
                           .long("config")
                           .value_name("FILE")
                           .multiple(true)
                           .takes_value(true))
                      .get_matches();
    println!("{:?}", matches);
```
### Steps to reproduce the issue

1. `cargo run -- -- test`

### Affected Version of `clap*`

2.33 

### Actual Behavior Summary

```
     Running `target/debug/x -- test`
error: The subcommand 'test' wasn't recognized
	Did you mean 'test'?

If you believe you received this message in error, try re-running with 'x -- test'

USAGE:
    x [OPTIONS] [SUBCOMMAND]

For more information try --help
```

### Expected Behavior Summary

clap parses a `test` subcommand. Or stops suggesting invalid invocations.

---

_Label `T: bug` added by @nagisa on 2020-03-04 16:53_

---

_Comment by @CreepySkeleton on 2020-03-04 17:02_

`-- subcommand` is definitely not how this is supposed to work. The error is confusing indeed, and we already thinking of an improvement in #1708 . I'm closing this issue in favor of that one.

---

_Closed by @CreepySkeleton on 2020-03-04 17:02_

---

_Comment by @ldm0 on 2020-03-05 10:35_

> `-- subcommand` is definitely not how this is supposed to work. The error is confusing indeed, and we already thinking of an improvement in #1708 . I'm closing this issue in favor of that one.

I think the problem is after using `--` we should not regard the `test` as a subcommand. I'd like to fix it. 

---

_Comment by @CreepySkeleton on 2020-03-05 10:38_

Go ahed

---

_Referenced in [clap-rs/clap#1727](../../clap-rs/clap/pulls/1727.md) on 2020-03-05 11:08_

---
