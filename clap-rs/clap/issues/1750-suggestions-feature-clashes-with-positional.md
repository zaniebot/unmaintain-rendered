---
number: 1750
title: Suggestions feature clashes with positional arguments value
type: issue
state: closed
author: kamilogorek
labels: []
assignees: []
created_at: 2020-03-17T12:59:04Z
updated_at: 2020-04-11T10:19:02Z
url: https://github.com/clap-rs/clap/issues/1750
synced_at: 2026-01-10T01:27:04Z
---

# Suggestions feature clashes with positional arguments value

---

_Issue opened by @kamilogorek on 2020-03-17 12:59_

### Rust Version

`rustc 1.42.0 (b8cedc004 2020-03-09)`

### Code

```toml
[dependencies]
clap = { git = "https://github.com/clap-rs/clap/" }
```

```rust
extern crate clap;

use clap::{App, Arg};

fn main() {
    let matches = App::new("clap-issue")
        .subcommand(App::new("yep").arg(Arg::with_name("version").required(true).index(1)))
        .subcommand(
            App::new("nope")
                .arg(Arg::with_name("version").required(true).index(1))
                .subcommand(App::new("list")),
        )
        .get_matches();

    if let Some(matches) = matches.subcommand_matches("yep") {
        println!("yep matched");
        if matches.is_present("version") {
            println!("{:?}", matches.value_of("version"));
            println!("version option found");
        } else {
            println!("version option not found");
        }
    }

    if let Some(matches) = matches.subcommand_matches("nope") {
        println!("nope matched");
        if matches.is_present("version") {
            println!("version option found");
        } else {
            println!("version option not found");
        }
    }
}
```
### Steps to reproduce the issue

```terminal
$ cargo run nope listversion list
```

### Affected Version of `clap*`

`master` and `2.33.0`

### Actual Behavior Summary

(#1 - #3 are working correctly)

```terminal
#4
$ cargo run nope listversion list // error: The subcommand 'xlistx' wasn't recognized. Did you mean 'list'?
$ cargo run nope xlist list // error: The subcommand 'xlistx' wasn't recognized. Did you mean 'list'?
$ cargo run nope xlistx list // error: The subcommand 'xlistx' wasn't recognized. Did you mean 'list'?
```

Affects: https://github.com/getsentry/sentry-cli/issues/686

### Expected Behavior Summary


```terminal
# 1
$ cargo run yep // missing required <version>
$ cargo run yep someversion // <version> someversion found

#2
$ cargo run nope // missing required <version>
$ cargo run nope someversion // <version> someversion found

#3
$ cargo run nope list // missing required <version>
$ cargo run nope someversion list // <version> someversion found

#4
$ cargo run nope listversion list // <version> listversion found
$ cargo run nope xlist list // <version> xlist found
$ cargo run nope xlistx list // <version> xlistx found
```

### Additional context

The code falls into this block and disregards the fact that there should be a positional argument present: https://github.com/clap-rs/clap/blob/9c8f9e20b989ff290a8942d5cfc2c6b8140f466f/src/parse/parser.rs#L545-L562

It works just fine when `suggestions` feature is turned off:
```toml
[dependencies]
clap = { git = "https://github.com/clap-rs/clap/", default-features = false, features = [ "std", "suggestions", "debug" ] }
```

or when one of `InferSubcommands` or `AllowExternalSubcommands` is used on `App::new("nope")`.

---

_Label `T: bug` added by @kamilogorek on 2020-03-17 12:59_

---

_Referenced in [getsentry/sentry-cli#686](../../getsentry/sentry-cli/issues/686.md) on 2020-03-17 12:59_

---

_Label `C: args` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `C: positional args` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-03-17 22:54_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-03-17 22:54_

---

_Label `W: 2.x` removed by @pksunkara on 2020-04-09 08:27_

---

_Label `W: 3.x` removed by @pksunkara on 2020-04-09 08:27_

---

_Comment by @pksunkara on 2020-04-09 08:29_

@pickfire Did your suggestions PR fix this issue?

---

_Comment by @pickfire on 2020-04-11 09:47_

I don't understand the problem.

---

_Comment by @pksunkara on 2020-04-11 10:17_

This is exactly the same issue as #878. Marking this as duplicate and closing. @pickfire Maybe you can understand the problem from that issue better.

---

_Label `C: args` removed by @pksunkara on 2020-04-11 10:17_

---

_Label `C: positional args` removed by @pksunkara on 2020-04-11 10:17_

---

_Label `C: subcommands` removed by @pksunkara on 2020-04-11 10:17_

---

_Label `P2: need to have` removed by @pksunkara on 2020-04-11 10:17_

---

_Label `T: bug` removed by @pksunkara on 2020-04-11 10:17_

---

_Label `R: duplicate` added by @pksunkara on 2020-04-11 10:17_

---

_Closed by @pksunkara on 2020-04-11 10:17_

---

_Comment by @pksunkara on 2020-04-11 10:19_

@kamilogorek Could you check that issue and try the options that are mentioned there?

---

_Referenced in [clap-rs/clap#878](../../clap-rs/clap/issues/878.md) on 2020-04-14 12:08_

---
