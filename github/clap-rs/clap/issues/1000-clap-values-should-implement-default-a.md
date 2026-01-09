---
number: 1000
title: "clap::Values should implement Default<'a> "
type: issue
state: closed
author: fulara
labels: []
assignees: []
created_at: 2017-07-16T17:08:51Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/1000
synced_at: 2026-01-07T13:12:19-06:00
---

# clap::Values should implement Default<'a> 

---

_Issue opened by @fulara on 2017-07-16 17:08_

Problem:
```
let f = matches.values_of("bla").unwrap(); //compiles
let f = matches.values_of("bla").unwrap_or(::clap::Values::default()); //does not compile
```
with error:
```
Error[E0597]: `matches` does not live long enough
  --> src/arguments.rs:68:13
   |
68 |     let f = matches.values_of("bla").unwrap_or(::clap::Values::default());
   |             ^^^^^^^ does not live long enough
...
89 | }
   | - borrowed value only lives until here
   |
   = note: borrowed value must be valid for the static lifetime...
```
Root cause:
```
impl Default for Values<'static>
```
### Rust Version
rustc 1.20.0-nightly (b2c070787 2017-07-13)
### Affected Version of clap
2.25.0


---

_Comment by @kbknapp on 2017-07-29 17:47_

Closed with #998 

---

_Closed by @kbknapp on 2017-07-29 17:47_

---
