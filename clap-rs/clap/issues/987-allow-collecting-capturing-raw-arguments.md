---
number: 987
title: "Allow collecting/capturing 'raw' arguments"
type: issue
state: closed
author: Aaron1011
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2017-06-11T18:47:46Z
updated_at: 2018-08-02T03:30:08Z
url: https://github.com/clap-rs/clap/issues/987
synced_at: 2026-01-10T01:26:40Z
---

# Allow collecting/capturing 'raw' arguments

---

_Issue opened by @Aaron1011 on 2017-06-11 18:47_

Issue https://github.com/kbknapp/clap-rs/issues/484 describes how to use clap to capture all arguments after a `--` delimiter. However, these arguments are still parsed by clap, which can lead to issues when creating a program which wraps a command-line tool.

For example, clap will condense multiple usages of a single argument (e.g. `-v -v -v`) into a single argument with multiple usages. However, this behavior is undesirable when wrapping another program, as the user normally intends to pass all arguments exactly as-is to the other program (which will perform its own argument parsing).

It would be useful if `clap::Arg` were to have a 'raw' option (which would require 'last' to be set). When enabled, this option would disable all parsing for the captured text, returning it to the API consumer as a single string.

---

_Comment by @kbknapp on 2017-06-16 15:41_

Using a combination of [`Arg::last`](https://docs.rs/clap/2.24.2/clap/struct.Arg.html#method.last), [`Arg::multiple`](https://docs.rs/clap/2.24.2/clap/struct.Arg.html#method.multiple), and [`Arg::allow_hyphen_values`](https://docs.rs/clap/2.24.2/clap/struct.Arg.html#method.allow_hyphen_values) you should be able to achieve what you're looking for. I'd also be OK with something like `Arg::raw(true)` which implicitly sets all the above settings for you as a convenience setting. But this will also be pretty low on the priority list as there are workarounds already, and I have quite a few other issues which are pressing 😉 

> returning it to the API consumer as a single string.

This the only part I'd be leary of. It's very easy to collect and join into a `String`, so I'd rather leave that to user code as once it's done it's done. Leaving it as an iterator of `Values` lets the consumer do with it as they will (iterate, collect, etc.).

If someone wants to take a swing at it, I'll be willing to mentor!

---

_Label `C: args` added by @kbknapp on 2017-06-16 15:41_

---

_Label `D: easy` added by @kbknapp on 2017-06-16 15:41_

---

_Label `M: mentored` added by @kbknapp on 2017-06-16 15:41_

---

_Label `P4: nice to have` added by @kbknapp on 2017-06-16 15:41_

---

_Label `T: enhancement` added by @kbknapp on 2017-06-16 15:41_

---

_Label `W: 2.x` added by @kbknapp on 2017-06-16 15:41_

---

_Comment by @Aaron1011 on 2017-06-16 16:28_

@kbknapp: It's my understanding the `Arg::multiple` would require the user to manually expand out the condensed string (e.g. changing 3 captured occurrences of `-v` back into `-v -v -v`). It's certainly doable, but it adds additional boilerplate to the process of wrapping a process.

> This the only part I'd be leary of. It's very easy to collect and join into a String,

That sounds fine to me - I just think that it should be possible to get such an iterator without needing to undo any processing done by clap (effectively getting a slice into `argv`).

---

_Comment by @kbknapp on 2017-06-16 20:18_

> It's my understanding the Arg::multiple would require the user to manually expand out the condensed string (e.g. changing 3 captured occurrences of -v back into -v -v -v). 

Nope, it'll happily accept multiples without parsing them:

```
kevin@spawn: ~/Projects/raw_test
➜ cat src/main.rs
extern crate clap;

use clap::{App, Arg};

fn main() {
    let m = App::new("raw_test")
        .arg(Arg::from_usage("--foo [val] 'just some opt'"))
        .arg(Arg::from_usage("-b, --bar 'just some flag'").multiple(true))
        .arg(Arg::from_usage("[raw] 'wraps raw args'")
            .multiple(true)
            .allow_hyphen_values(true)
            .last(true))
        .get_matches();
    
    println!("foo: {:?}", m.value_of("foo"));
    println!("bar: {:?}", m.is_present("bar"));
    println!("raw args: {:?}", m.values_of("raw").unwrap().collect::<Vec<_>>().join(" "));
}

kevin@spawn: ~/Projects/raw_test
➜ ./target/debug/raw_test --foo something -- -v -v -v -b -b -b --baz -q -u -x
foo: Some("something")
bar: false
raw args: "-v -v -v -b -b -b --baz -q -u -x"
```

Notice after the `--` it didn't try to parse `-b` even though that's a valid arg for this test.

---

_Referenced in [clap-rs/clap#1188](../../clap-rs/clap/pulls/1188.md) on 2018-02-19 17:58_

---

_Comment by @kbknapp on 2018-02-23 21:00_

Closed with #1191 

---

_Closed by @kbknapp on 2018-02-23 21:00_

---
