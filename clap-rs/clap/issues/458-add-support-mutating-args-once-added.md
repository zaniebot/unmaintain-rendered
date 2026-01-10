---
number: 458
title: Add support mutating args once added
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - E-hard
assignees: []
created_at: 2016-03-23T02:06:40Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/458
synced_at: 2026-01-10T01:26:29Z
---

# Add support mutating args once added

---

_Issue opened by @kbknapp on 2016-03-23 02:06_

It'd be nice to be able to mutate aspects of certain args after they've been add to an `App` struct.

What I'd like to be able to do is add a bunch of args via the `App::args_from_usage` then later go back and mutate the two or three outlyers that require some special setting, like `possible_values` or something.


---

_Label `T: enhancement` added by @kbknapp on 2016-03-23 02:07_

---

_Label `C: args` added by @kbknapp on 2016-03-23 02:07_

---

_Label `D: intermediate` added by @kbknapp on 2016-03-23 02:07_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-23 02:07_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-23 02:07_

---

_Comment by @joshtriplett on 2016-06-24 07:18_

One possible interface to this would be a new `App` method `mod_arg<F: FnMut(Arg) -> Arg>(name: &str, f: F)`:

``` rust
let app = App::new("cmd").args_from_usage("...");
app.mod_arg("somearg", |arg| arg.possible_values(...));
```


---

_Label `D: hard` added by @kbknapp on 2016-11-02 03:00_

---

_Label `D: intermediate` removed by @kbknapp on 2016-11-02 03:00_

---

_Label `W: 3.x` added by @kbknapp on 2017-04-05 18:37_

---

_Label `W: 2.x` removed by @kbknapp on 2017-04-05 18:37_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-22 04:16_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:51_

---

_Comment by @kbknapp on 2018-07-22 00:49_

Has been implemented on v3-master

---

_Closed by @kbknapp on 2018-07-22 00:49_

---
