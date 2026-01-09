---
number: 1092
title: Fatal internal error when using requires outside subcommand boundary
type: issue
state: closed
author: clux
labels: []
assignees: []
created_at: 2017-11-04T16:09:35Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1092
synced_at: 2026-01-07T13:12:19-06:00
---

# Fatal internal error when using requires outside subcommand boundary

---

_Issue opened by @clux on 2017-11-04 16:09_

There seems to be other similar issue related to `requires`, but just posting this simple one which seems to have a slightly different `SubCommand` origin:

```rust
extern crate clap;
use clap::{Arg, App, SubCommand};

fn main() {
    let app = App::new("bah")
        .arg(Arg::with_name("environment")
            .short("e")
            .takes_value(true))
       .subcommand(SubCommand::with_name("query")
            .arg(Arg::with_name("component")
                .required(true)
                .requires("environment")));
    let _ = app.get_matches();
}
```

The `requires` call inside the subcommand is what's causing the crash when you try to use the `query` subcommand on  your executable.

Rust 1.21.0, clap 2.27.1

---

_Comment by @clux on 2017-11-04 16:16_

I thought it may have been related to the double require (I really wanted a `.requires` method on the `SubCommand` itself), but it seems to crash even without that. But with the double require, you can't even view the help without it crashing.

---

_Comment by @kbknapp on 2017-11-06 03:30_

clap treats `bah` and `query` as separate entities. There isn't currently a way to require that a parent command's argument was used. I would recommend either using [`Arg::global(true)`](https://docs.rs/clap/2.27.1/clap/struct.Arg.html#method.global) on `environment` or moving it into the `query` subcommand.

The `panic` is by design. If it did output the help message, this error could go undetected. `environment` doesn't actually exist as far as `query` is concerned.

---

_Label `T: RFC / question` added by @kbknapp on 2017-11-06 03:38_

---

_Comment by @kbknapp on 2017-11-06 03:47_

I'm going to close this for the reasons above, however I'm open to discussion if there is something that could be done better :wink:

---

_Closed by @kbknapp on 2017-11-06 03:47_

---

_Comment by @clux on 2017-11-06 15:00_

Yeah, I think that makes sense. Didn't know there was a way to push it into each subcommand with `global`, that's a nicer solution that works. Thank you.

---
