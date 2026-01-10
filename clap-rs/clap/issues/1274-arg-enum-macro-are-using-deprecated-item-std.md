---
number: 1274
title: "arg_enum! macro are using deprecated item 'std::ascii::AsciiExt'"
type: issue
state: closed
author: cathay4t
labels: []
assignees: []
created_at: 2018-05-21T17:50:29Z
updated_at: 2018-08-02T03:30:24Z
url: https://github.com/clap-rs/clap/issues/1274
synced_at: 2026-01-10T01:26:47Z
---

# arg_enum! macro are using deprecated item 'std::ascii::AsciiExt'

---

_Issue opened by @cathay4t on 2018-05-21 17:50_

### Rust Version

rustc 1.26.0 (a77568041 2018-05-07)
Installed by `rustup` stable.

### Affected Version of clap

clap 2.31.2

### Expected Behavior Summary

No warnings on `arg_enum!`.

### Actual Behavior Summary

Got warning of deprecated item 'std::ascii::AsciiExt'

```
warning: use of deprecated item 'std::ascii::AsciiExt': use inherent methods instead
  --> src/main.rs:21:1
   |
21 | / arg_enum!{
22 | |     #[derive(Debug)]
23 | |     pub enum Oof {
24 | |         Rab,
...  |
27 | |     }
28 | | }
   | |_^
   |
   = note: #[warn(deprecated)] on by default
   = note: this error originates in a macro outside of the current crate
 (in Nightly builds, run with -Z external-macro-backtrace for more info)

```

### Steps to Reproduce the issue
```bash
cd /tmp
cargo new --bin bug_test
cd bug_test
wget https://github.com/kbknapp/clap-rs/raw/master/examples/13a_enum_values_automatic.rs -O ./src/main.rs
echo 'clap = "2.31.2"' >> Cargo.toml
cargo build
```

### Sample Code or Link to Sample Code

https://github.com/kbknapp/clap-rs/blob/master/examples/13a_enum_values_automatic.rs

### Debug output

https://gist.github.com/cathay4t/ac4948b602c269689cab4c2b5bb2e6cd

---

_Comment by @coder543 on 2018-05-24 01:21_

It looks like this has already [been fixed.](https://github.com/kbknapp/clap-rs/commit/b04a3b9ac97925a64c1dce6981359f7796cd9fc2#diff-e1576943a5907b15388ab726037ebdb3) Someone who can do a new release just needs to do a new release.

---

_Comment by @kbknapp on 2018-06-05 01:14_

Sorry I've been out for a few weeks, I'll be putting out the new version this week :wink: 

---

_Closed by @kbknapp on 2018-06-05 01:14_

---

_Comment by @DarrenTsung on 2018-06-13 22:58_

Hey @kbknapp, any update on getting a new version out? Would love to have a warning-free compile!

---

_Comment by @kbknapp on 2018-06-22 15:48_

@DarrenTsung apologies! I've been wanting to get it out, but keep waiting on "one last bug fix" to merge :stuck_out_tongue_winking_eye: Once #1301 merges I'll put out the new version. I promise! :pray: 

---

_Comment by @DarrenTsung on 2018-06-22 16:59_

No worries, thanks for all your hard work :).

---
