---
number: 1800
title: "Regression in 3.x: trailing commas do not mix well with arg_enum!"
type: issue
state: closed
author: lf-
labels:
  - C-bug
assignees: []
created_at: 2020-04-09T09:51:04Z
updated_at: 2020-04-10T08:41:25Z
url: https://github.com/clap-rs/clap/issues/1800
synced_at: 2026-01-07T13:12:19-06:00
---

# Regression in 3.x: trailing commas do not mix well with arg_enum!

---

_Issue opened by @lf- on 2020-04-09 09:51_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

Hi! It looks like #1244 got lost in the move to v3.x. Based on my poking at the git log, it seems like it just never made it into the history of `master`. #1097 also. Below is a bug report for the bug itself:

### Rust Version

`rustc 1.44.0-nightly (485c5fb6e 2020-04-08)`

### Code

```rust
use clap::arg_enum;

arg_enum! {
enum Meow {
    Raw,
    Quoted,
}
}
```

### Steps to reproduce the issue

1. `cargo new`; add the dependency to `Cargo.toml`
2. paste the above into the top of `src/main.rs`
3. `cargo build`

### Affected Version of `clap*`

`3.0.0-beta.1` (I have `clap = { git = "https://github.com/clap-rs/clap/" }` in my Cargo.toml)

### Actual Behavior Summary

```
error: no rules expected the token `:`
   --> C:\Users\lf\.cargo\git\checkouts\clap-78dbe9b58f9073fe\89a4764\src\macros.rs:401:34
    |
317 | / macro_rules! arg_enum {
318 | |     (@as_item $($i:item)*) => ($($i)*);
319 | |     (@impls ( $($tts:tt)* ) -> ($e:ident, $($v:ident),+)) => {
320 | |         $crate::arg_enum!(@as_item
...   |
401 | |         $crate::arg_enum!(enum $e:ident {
    | |                                  ^ no rules expected this token in macro call
...   |
411 | |     };
412 | | }
    | |_- in this expansion of `arg_enum!`
    | 
   ::: src\main.rs:21:1
    |
21  | / arg_enum! {
22  | | enum Meow {
23  | |     Raw,
24  | |     Quoted,
25  | | }
26  | | }
    | |_- in this macro invocation
```

### Expected Behavior Summary

I'd like it to compile because trailing commas are nice :)

### Additional context

N/A.


---

_Label `T: bug` added by @lf- on 2020-04-09 09:51_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 10:08_

---

_Comment by @pksunkara on 2020-04-09 10:09_

Would appreciate if you can make a PR cherry picking those commits.

---

_Comment by @pksunkara on 2020-04-09 23:15_

#1124 which is fix for #1067 is in v3.

---

_Comment by @lf- on 2020-04-09 23:19_

That's super strange since I tried the above steps which are mentioned in the readme as how to install the latest development version and it definitely isn't doing trailing commas. That commit *is* in master_orig but I assume there was some restructuring since then that could have excluded it?

---

_Comment by @pksunkara on 2020-04-09 23:34_

No, I meant, I checked the code and that code is in v3.

---

_Referenced in [clap-rs/clap#1802](../../clap-rs/clap/pulls/1802.md) on 2020-04-09 23:36_

---

_Closed by @bors[bot] on 2020-04-10 08:41_

---
