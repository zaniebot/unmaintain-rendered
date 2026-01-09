---
number: 831
title: required_if and and default values
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-01-30T02:10:59Z
updated_at: 2018-08-02T03:30:00Z
url: https://github.com/clap-rs/clap/issues/831
synced_at: 2026-01-07T13:12:19-06:00
---

# required_if and and default values

---

_Issue opened by @kbknapp on 2017-01-30 02:10_

From #824 

```rust
extern crate clap;

use clap::{Arg, App};

fn main() {
    let m = App::new("Test app")
        .version("1.0")
        .author("F0x06")
        .about("Arg test")
        .arg(Arg::with_name("target")
            .takes_value(true)
            .default_value("file")
            .possible_values(&["file", "stdout"])
            .long("target"))
        .arg(Arg::with_name("input")
            .takes_value(true)
            .required(true)
            .long("input"))
        .arg(Arg::with_name("output")
            .takes_value(true)
            .required_if("target", "file")
            .long("output"))
        .get_matches_from_safe(vec!["test", "--input", "some"]);

    assert!(m.is_err());
    assert_eq!(m.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
}
```



---

_Label `C: parsing` added by @kbknapp on 2017-01-30 02:11_

---

_Label `D: intermediate` added by @kbknapp on 2017-01-30 02:11_

---

_Label `P2: need to have` added by @kbknapp on 2017-01-30 02:11_

---

_Label `T: bug` added by @kbknapp on 2017-01-30 02:11_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-30 02:11_

---

_Added to milestone `2.20.2` by @kbknapp on 2017-01-30 04:36_

---

_Added to milestone `2.20.3` by @kbknapp on 2017-02-03 16:12_

---

_Removed from milestone `2.20.2` by @kbknapp on 2017-02-03 16:12_

---

_Closed by @kbknapp on 2017-02-03 22:43_

---
