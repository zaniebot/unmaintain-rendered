---
number: 192
title: "no suggestion for possible value when using 'long=' format"
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2015-08-27T02:29:57Z
updated_at: 2018-08-02T03:29:42Z
url: https://github.com/clap-rs/clap/issues/192
synced_at: 2026-01-10T01:26:25Z
---

# no suggestion for possible value when using 'long=' format

---

_Issue opened by @kbknapp on 2015-08-27 02:29_

When setting up a long option with possible values, and you run `--long=value` if `value` isn't valid you will not get an error. Yet running `--long value` will result in the appropriate error.

Example test:

``` rust
static VALUES: [&'static str; 2] = ["possible", "vals"];

fn main() {
    App::new("test").arg(Arg::from_usage("--option <val> 'some option'").possible_values(&VALUES)).get_matches();
}
```


---

_Label `T: bug` added by @kbknapp on 2015-08-27 02:29_

---

_Label `P1: urgent` added by @kbknapp on 2015-08-27 02:29_

---

_Label `C: args` added by @kbknapp on 2015-08-27 02:29_

---

_Label `D: easy` added by @kbknapp on 2015-08-27 02:29_

---

_Label `C: parsing` added by @kbknapp on 2015-08-27 02:29_

---

_Closed by @kbknapp on 2015-08-27 03:54_

---
