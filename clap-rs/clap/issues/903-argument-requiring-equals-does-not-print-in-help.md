---
number: 903
title: Argument requiring equals does not print = in help
type: issue
state: closed
author: TimBednarzyk
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2017-03-14T21:33:28Z
updated_at: 2018-08-02T03:30:04Z
url: https://github.com/clap-rs/clap/issues/903
synced_at: 2026-01-10T01:26:38Z
---

# Argument requiring equals does not print = in help

---

_Issue opened by @TimBednarzyk on 2017-03-14 21:33_

### Rust Version

1.14.0

### Affected Version of clap

2.21.1

### Expected Behavior Summary

When calling `arg.require_equals(true)`, the generated help message should show `--arg=<VALUE>`. Without the equals sign in the help message, the user will think to put a space and will be confused when that fails.

### Actual Behavior Summary

The help message shows `--arg <VALUE>`

### Sample Code

```
extern crate clap;

use clap::*;

fn main() {
  let matches = App::new("example")
    .arg(Arg::with_name("mode")
         .long("mode")
         .value_name("MODE")
         .require_equals(true)
         .default_value("foo")
         .possible_values(&["foo", "bar"])
         ).get_matches();

  if matches.value_of("mode").unwrap() == "foo" {
    println!("mode == foo");
  }
  else {
    println!("mode == bar");
  }
}
```

### Debug output

![image](https://cloud.githubusercontent.com/assets/8797125/23923006/27647fbe-08db-11e7-9273-86051dca9acb.png)

---

_Comment by @kbknapp on 2017-03-15 03:13_

Great point, I hadn't thought about that. This is a very simple fix. I'm away from my computer for a few days, but will knock this out first thing when I return. If anyone submits a PR, it's simply adding some lines to the Display implementation of OptBuilder.

---

_Label `C: options` added by @kbknapp on 2017-03-15 03:16_

---

_Label `D: easy` added by @kbknapp on 2017-03-15 03:16_

---

_Label `M: mentored` added by @kbknapp on 2017-03-15 03:16_

---

_Label `P3: want to have` added by @kbknapp on 2017-03-15 03:16_

---

_Label `T: enhancement` added by @kbknapp on 2017-03-15 03:16_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-15 03:16_

---

_Closed by @homu on 2017-03-17 02:43_

---
