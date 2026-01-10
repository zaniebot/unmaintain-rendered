---
number: 971
title: "Example supporting a \"slop\" style parse"
type: issue
state: closed
author: drusellers
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2017-05-27T17:26:05Z
updated_at: 2018-08-02T03:30:07Z
url: https://github.com/clap-rs/clap/issues/971
synced_at: 2026-01-10T01:26:40Z
---

# Example supporting a "slop" style parse

---

_Issue opened by @drusellers on 2017-05-27 17:26_

Would it be possible to get an example of using the `--` argument parsing concept?

`my-command -f -p bob -- a whole bunch of stuff`

Is there a way to do this already in clap? Or should I write up a feature request?

---

_Comment by @kbknapp on 2017-05-29 18:42_

Absolutely, it's already possible but adding a new example would be great!

You can add the example to the `examples/` directory, and I'd call it something like "stop_parsing_with_--" or similar.

A quick example is:

```rust
extern crate clap;

use clap::{App, Arg};

fn main() {
    let matches = App::new("myprog")
        .arg(Arg::with_name("eff")
            .short("f"))
        .arg(Arg::with_name("pea")
             .short("p")
             .takes_value(true))
        .arg(Arg::with_name("slop")
             .multiple(true))
        .get_matches();

    println!("-f used: {:?}", matches.is_present("eff"));
    println!("-p's value: {:?}", matches.value_of("pea"));
    println!("'slops' values: {:?}", matches.values_of("slop").map(|vals| vals.collect::<Vec<_>>()));
}
```

---

_Label `C: examples` added by @kbknapp on 2017-05-29 18:42_

---

_Label `D: easy` added by @kbknapp on 2017-05-29 18:42_

---

_Label `M: mentored` added by @kbknapp on 2017-05-29 18:42_

---

_Label `P4: nice to have` added by @kbknapp on 2017-05-29 18:42_

---

_Label `T: enhancement` added by @kbknapp on 2017-05-29 18:42_

---

_Comment by @drusellers on 2017-05-30 12:26_

A: Awesome - I have it working.
B: My current use case looks like `my-command rc abc def -- ping google.com`
`<PROGRAM> <COMMAND> <POS ARG...> -- <SLOP...>

Clap doesn't seem to know about `--` as it is saying that two multiple args are no bueno. I'll be adding the example shortly, but I wonder if we couldn't teach clap about `--`

---

_Referenced in [clap-rs/clap#977](../../clap-rs/clap/pulls/977.md) on 2017-05-31 19:42_

---

_Closed by @kbknapp on 2017-06-10 21:38_

---

_Comment by @messense on 2017-06-19 13:11_

Is it possible to make the `--` mandatory for the slop args?

---

_Comment by @drusellers on 2017-06-19 13:11_

I too would like that. :)

---

_Comment by @messense on 2017-06-19 13:13_

Without that it would be a break change if I add some other args in the future before the `--`.

---

_Comment by @kbknapp on 2017-06-19 17:05_

@messense and @drusellers 

Look into using [`Arg::last`](https://docs.rs/clap/2.24.2/clap/struct.Arg.html#method.last) it does what you're looking for 😄 

---

_Referenced in [clap-rs/clap#991](../../clap-rs/clap/pulls/991.md) on 2017-06-23 05:50_

---

_Referenced in [commander-rb/commander#96](../../commander-rb/commander/issues/96.md) on 2020-07-02 13:55_

---
