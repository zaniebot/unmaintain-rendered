---
number: 470
title: "empty values aren't stored"
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-03-30T00:48:25Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/470
synced_at: 2026-01-10T01:26:29Z
---

# empty values aren't stored

---

_Issue opened by @kbknapp on 2016-03-30 00:48_

``` rust
extern crate clap;

use clap::{App, AppSettings};

fn main() {
    let m = App::new("test")
        .setting(AppSettings::TrailingVarArg)
        .arg_from_usage("<command>... 'some feature'")
        .get_matches();

    for c in m.values_of("command").unwrap() {
        println!("{}", c);
    }

    println!("len: {}", m.values_of("command").unwrap().collect::<Vec<_>>().len());

}
```

Running with 

```
$ test ""
len: 0
```


---

_Label `T: bug` added by @kbknapp on 2016-03-30 00:48_

---

_Label `P2: need to have` added by @kbknapp on 2016-03-30 00:48_

---

_Label `C: args` added by @kbknapp on 2016-03-30 00:48_

---

_Label `D: easy` added by @kbknapp on 2016-03-30 00:48_

---

_Label `C: parsing` added by @kbknapp on 2016-03-30 00:48_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-30 00:48_

---

_Closed by @homu on 2016-03-30 03:26_

---
