---
number: 420
title: Positional Args with Value Names prints Arg Name
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
  - E-medium
assignees: []
created_at: 2016-02-08T21:22:15Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/420
synced_at: 2026-01-07T13:12:19-06:00
---

# Positional Args with Value Names prints Arg Name

---

_Issue opened by @kbknapp on 2016-02-08 21:22_

``` rust
extern crate clap;

use clap::{App, Arg};

fn main() {
    let m = App::new("valnames")
        .arg(Arg::with_name("arg")
            .required(true)
            .value_name("FILE"))
        .get_matches();
}
```

Help prints:

```
$ valnames --help
valnames 

USAGE:
    valnames [FLAGS] [ARGS] <arg>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    arg    

```

Note `arg`


---

_Label `T: enhancement` added by @kbknapp on 2016-02-08 21:23_

---

_Label `C: args` added by @kbknapp on 2016-02-08 21:23_

---

_Label `C: help message` added by @kbknapp on 2016-02-08 21:23_

---

_Label `D: easy` added by @kbknapp on 2016-02-08 21:23_

---

_Label `P3: want to have` added by @kbknapp on 2016-02-08 21:23_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-08 21:23_

---

_Label `M: mentored` added by @kbknapp on 2016-02-08 21:23_

---

_Referenced in [clap-rs/clap#422](../../clap-rs/clap/pulls/422.md) on 2016-02-09 18:30_

---

_Closed by @homu on 2016-02-09 19:30_

---
