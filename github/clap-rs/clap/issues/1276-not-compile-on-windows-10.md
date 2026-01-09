---
number: 1276
title: not compile on windows 10
type: issue
state: closed
author: josselinchevalay
labels: []
assignees: []
created_at: 2018-05-23T10:05:00Z
updated_at: 2018-08-02T03:30:24Z
url: https://github.com/clap-rs/clap/issues/1276
synced_at: 2026-01-07T13:12:19-06:00
---

# not compile on windows 10

---

_Issue opened by @josselinchevalay on 2018-05-23 10:05_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.18.0 (03fc9d622 2017-06-06)

### Affected Version of clap

clap 2.31.2

### Expected Behavior Summary
compile on windows 10 

### Actual Behavior Summary
i have this error 

```powershell
error: expected ident, found #
  --> C:\Users\jchevalay\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.31.2\src\app\settings.rs:7:1
   |
7  | / bitflags! {
8  | |     struct Flags: u64 {
9  | |         const SC_NEGATE_REQS       = 1;
10 | |         const SC_REQUIRED          = 1 << 1;
...  |
50 | |     }
51 | | }
   | |_^
   |
   = note: this error originates in a macro outside of the current crate

error: Could not compile `clap`.
``` 

### Steps to Reproduce the issue
use cargo build

### Sample Code or Link to Sample Code

```rust
extern crate clap;
use clap::App;

fn main() {
   App::new("hello world")
  .get_matches();
}

```
### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[package]
name = "hello-cli"
version = "0.1.0"
authors = ["Josselin Chevalay <josselin.chevalay@ullink.com>"]

[dependencies]
clap = "~2.31.2"
```
The output may be very long, so feel free to link to a gist or attach a text file


---

_Comment by @kbknapp on 2018-05-23 22:01_

The minimum required rustc is 1.20, I'm not sure if that's the issue here or not, but could you try building again with that version of rustc or greater? I'm not at a computer at the moment, but once I am I can dig into this more.

---

_Comment by @josselinchevalay on 2018-05-24 06:50_

ok i will upgrade my rustc 

---

_Comment by @josselinchevalay on 2018-05-24 07:16_

yes works fine thx a lot ! 

---

_Closed by @josselinchevalay on 2018-05-24 07:16_

---
