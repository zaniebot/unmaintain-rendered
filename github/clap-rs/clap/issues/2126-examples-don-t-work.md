---
number: 2126
title: "Examples don't work"
type: issue
state: closed
author: azzamsa
labels:
  - A-docs
assignees: []
created_at: 2020-09-08T05:31:07Z
updated_at: 2020-11-09T02:06:54Z
url: https://github.com/clap-rs/clap/issues/2126
synced_at: 2026-01-07T13:12:19-06:00
---

# Examples don't work

---

_Issue opened by @azzamsa on 2020-09-08 05:31_

### Clap version

[dependencies]
clap = "3.0.0-beta.1"


### Where

https://github.com/clap-rs/clap/blob/v2.33.1/examples/04_using_matches.rs

### What's wrong

```rust
   Compiling loye v0.1.0 (/home/user/app)
error[E0308]: mismatched types
  --> src/main.rs:26:24
   |
26 |                 .short("d")
   |                        ^^^ expected `char`, found `&str`

error[E0308]: mismatched types
  --> src/main.rs:32:24
   |
32 |                 .short("c")
   |                        ^^^ expected `char`, found `&str`

error: aborting due to 2 previous errors

For more information about this error, try `rustc --explain E0308`.
error: could not compile `loye`.

To learn more, run the command again with --verbose.

rust-compilation exited abnormally with code 101 at Tue Sep  8 12:28:43
```

---

_Label `C: docs` added by @azzamsa on 2020-09-08 05:31_

---

_Comment by @azzamsa on 2020-09-08 05:33_

solved by changing double quotes to *single-quotes*.

Should I make a PR?

---

_Closed by @azzamsa on 2020-09-08 05:33_

---

_Reopened by @azzamsa on 2020-09-08 05:33_

---

_Comment by @CreepySkeleton on 2020-09-08 15:32_

> clap = "***3.0.0-beta.1***"
>
> github.com/clap-rs/clap/blob/***v2.33.1***/examples/04_using_matches.rs

You're trying to compile `2.x` examples with `3.x` version and you're failing. Water is wet.

---

_Closed by @CreepySkeleton on 2020-09-08 15:32_

---
