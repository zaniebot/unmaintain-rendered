```yaml
number: 769
title: "Don't collapse optional positional arguments into `[ARGS]` in the usage string"
type: issue
state: closed
author: dotdash
labels:
  - A-help
assignees: []
created_at: 2016-12-07T14:11:25Z
updated_at: 2016-12-31T06:15:13Z
url: https://github.com/clap-rs/clap/issues/769
synced_at: 2026-01-12T16:14:09Z
```

# Don't collapse optional positional arguments into `[ARGS]` in the usage string

---

_@dotdash_

### Rust Version

rustc 1.15.0-nightly (daf8c1dfc 2016-12-05)

### Affected Version of clap

2.19.1

### Expected Behavior Summary

If I have one required and two optional positional arguments, I expect the usage string to say:

```
USAGE:
    clap-test <FILE> [<WIDTH> [<HEIGHT>]]
```

to properly represent the order in which the arguments need to be given.

### Actual Behavior Summary

The optional positional arguments are collapsed into a single `[ARGS]` placeholder, and I have to look at the `ARGS` block and know it tells me the required order.

```
USAGE:
    clap-test <FILE> [ARGS]

FLAGS:
        --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <FILE>      the configuration file
    <WIDTH>     texture width [default: 512]
    <HEIGHT>    texture height [default: 512]
```

### Steps to Reproduce the issue

Run the program with `--help`.

### Sample Code or Link to Sample Code

```rust
extern crate clap;
use clap::{Arg, App};

fn main() {
    let matches = App::new("My  app")
        .version("0.1")
        .author("Me <me@ma.il>")
        .about("Does stuff")
        .arg(Arg::with_name("config")
             .value_name("FILE")
             .help("the configuration file")
             .required(true)
             .takes_value(true)
             .index(1))
        .arg(Arg::with_name("width")
             .value_name("WIDTH")
             .help("texture width")
             .takes_value(true)
             .default_value("512")
             .index(2))
        .arg(Arg::with_name("height")
             .value_name("HEIGHT")
             .help("texture height")
             .takes_value(true)
             .default_value("512")
             .index(3))
        .get_matches();
}
```


---

_Comment by @kbknapp on 2016-12-07 20:01_

I do agree, but at the same time I worry about applications that have lots of positional arguments. I'd like to get some others opinions on this issue before merging.

One point I'd like to change from your proposal though is the `$ prog <required> [<optional1> <optional2>]`

I'd prefer that go to `$ prog <required> [optional1] [optional2]`. This is personal preference but I dislike the CLIs that give me a usage strings like `$ prog <opt> [[<req>|[-e <some>]] | [other]]` It gests difficult to parse (mentally) very quickly.

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-07 20:01_

---

_Comment by @dotdash on 2016-12-08 11:00_

Oh, I meant to mention that I'd be fine with not nesting the square brackets. Not sure what the best option is for handling lots of optional positional arguments, but the same problem already exists for required positional arguments, so a common solution would be nice.

---

_Comment by @kbknapp on 2016-12-08 14:51_

So we already don't collapse them down if it's only 2 positional arguments (I believe...I can't remember :stuck_out_tongue_winking_eye: ). So it's entirely possible that we only collapse them after only after a [higher] certain number (perhaps up the number to...5?).

---

_Comment by @dotdash on 2016-12-09 09:43_

They're collapsed to [ARGS] if there are two or more. I guess I could live with collapsing only if there are more than X items, or even just an option that allows me to just disable it altogether.

---

_Comment by @kbknapp on 2016-12-09 14:39_

Adding an option to just disable the collapsing is easy, and I think the route I'll go since that won't mess with current usage strings (i.e. it's an opt-in feature).

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-10 04:36_

---

_Label `C: help message` added by @kbknapp on 2016-12-10 04:36_

---

_Label `D: easy` added by @kbknapp on 2016-12-10 04:36_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-10 04:36_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-10 04:36_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-10 04:36_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-12-10 04:36_

---

_Label `T: new setting` added by @kbknapp on 2016-12-31 02:09_

---

_Label `T: enhancement` removed by @kbknapp on 2016-12-31 02:09_

---

_Assigned to @kbknapp by @kbknapp on 2016-12-31 02:09_

---

_Closed by @homu on 2016-12-31 06:15_

---

_Referenced in [clap-rs/clap#4151](../../clap-rs/clap/pulls/4151.md) on 2022-08-30 21:13_

---
