---
number: 461
title: "Positional argument before \"--\" in usage"
type: issue
state: closed
author: mbudde
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-03-27T15:01:46Z
updated_at: 2016-03-27T20:30:37Z
url: https://github.com/clap-rs/clap/issues/461
synced_at: 2026-01-10T01:26:29Z
---

# Positional argument before "--" in usage

---

_Issue opened by @mbudde on 2016-03-27 15:01_

``` rust
extern crate clap;

use clap::{App};

fn main() {
    App::new("Clap test")
        .arg_from_usage("<INPUT>...")
        .get_matches();
}
```

```
$ target/debug/clap-test -h
Clap test

USAGE:
    clap-test [FLAGS] <INPUT>... [--]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <INPUT>...    
```

I would expect usage to show `clap-test [FLAGS] [--] <INPUT>...`.


---

_Comment by @kbknapp on 2016-03-27 16:55_

Ah, thanks for filing this! It's a bug.

Actually in your example, the `[--]` shouldn't be there at all. The `[--]` only appears in the usage string when there are options which can accept multiple values, _and_ positional arguments where aren't required. This is in order to prompt the user and potentially prevent something like 

```
$ myprog --option val1 val2 "am I positional1 or val3?"
```


---

_Label `T: bug` added by @kbknapp on 2016-03-27 16:55_

---

_Label `P2: need to have` added by @kbknapp on 2016-03-27 16:55_

---

_Label `C: args` added by @kbknapp on 2016-03-27 16:55_

---

_Label `C: help message` added by @kbknapp on 2016-03-27 16:55_

---

_Label `D: easy` added by @kbknapp on 2016-03-27 16:55_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-27 16:55_

---

_Referenced in [clap-rs/clap#462](../../clap-rs/clap/pulls/462.md) on 2016-03-27 16:56_

---

_Assigned to @kbknapp by @kbknapp on 2016-03-27 20:04_

---

_Added to milestone `2.2.2` by @kbknapp on 2016-03-27 20:04_

---

_Closed by @kbknapp on 2016-03-27 20:29_

---

_Comment by @kbknapp on 2016-03-27 20:30_

v2.2.2 on crates.io should have fixed this.


---
