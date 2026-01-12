```yaml
number: 774
title: Usage line is wrong in case of errors when using default values
type: issue
state: closed
author: dotdash
labels:
  - C-enhancement
assignees: []
created_at: 2016-12-12T16:53:32Z
updated_at: 2018-08-02T03:29:58Z
url: https://github.com/clap-rs/clap/issues/774
synced_at: 2026-01-12T16:14:09Z
```

# Usage line is wrong in case of errors when using default values

---

_@dotdash_


### Affected Version of clap

2.19.2

### Expected Behavior Summary

The `USAGE` output is always the same, regardless of whether the user supplies `--help` or makes a usage error.

### Actual Behavior Summary

Usage with `--help`:

```
USAGE:
    render <CONFIG> [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <CONFIG>    the configuration file
    <WIDTH>     texture width [default: 512]
    <HEIGHT>    texture height [default: 512]
```

Usage with a bad input:

```
error: The following required arguments were not provided:
    <CONFIG>

USAGE:
    render <CONFIG> <WIDTH> <HEIGHT>

For more information try --help
```

Note how `<WIDTH>` and `<HEIGHT>` are shown as required arguments.

### Steps to Reproduce the issue

Call the executable created from the code below once with `--help` and once without any arguments.

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
             .value_name("CONFIG")
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

_Comment by @kbknapp on 2016-12-18 20:19_

@dotdash apologies for the long time in answering this, I've been out of town for work.

In that usage line `<width>` and `<height>` are not shown as required arguments, they're shown as *used* arguments (a default is considered used). When `clap` detects an argument was used (required or not) it will inject those arguments into the usage string of errors. It's a manner of saying, "It seems like you were trying to do this... X ... but here's how you can correct it."

I agree this could be improved by not displaying arguments which only contain the default value. I can keep this on the issue tracker, but there are a few more pressing issues to fix first ;)

There's also the case of it'd be nice to inject the actual value the users used for those args (non-defaults only). I should be able to start knocking this out after the new year.

---

_Label `C: errors` added by @kbknapp on 2016-12-18 20:20_

---

_Label `D: intermediate` added by @kbknapp on 2016-12-18 20:20_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-18 20:20_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-18 20:20_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-18 20:20_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-27 04:37_

---

_Closed by @homu on 2017-01-03 01:19_

---
