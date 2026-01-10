---
number: 5385
title: Can not compile clap success  when I use toolchain nightly-x86_64-unknown-linux-gnu
type: issue
state: closed
author: jokemanfire
labels:
  - C-bug
assignees: []
created_at: 2024-03-06T02:32:08Z
updated_at: 2024-03-07T01:33:53Z
url: https://github.com/clap-rs/clap/issues/5385
synced_at: 2026-01-10T01:28:11Z
---

# Can not compile clap success  when I use toolchain nightly-x86_64-unknown-linux-gnu

---

_Issue opened by @jokemanfire on 2024-03-06 02:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.74.0-nightly

### Clap Version

clap_lex-0.7.0

### Minimal reproducible code

cargo install cargo-outdated


### Steps to reproduce the bug with the above code

cargo install cargo-outdated

### Actual Behaviour

error[E0599]: no function or associated item named `from_encoded_bytes_unchecked` found for struct `OsStr` in the current scope
   -->***/clap_lex-0.7.0/src/ext.rs:234:24
    |
234 |                 OsStr::from_encoded_bytes_unchecked(second),
    |                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ function or associated item not found in `OsStr`


### Expected Behaviour

I expected the clap should compile successful

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @jokemanfire on 2024-03-06 02:32_

---

_Comment by @epage on 2024-03-06 03:25_

While our MSRV is 1.74, you are likely on a 1.74 nightly from before this feature was stabilized.

---

_Comment by @jokemanfire on 2024-03-06 03:56_

> While our MSRV is 1.74, you are likely on a 1.74 nightly from before this feature was stabilized.

thank you, but it means clap didn't support for 1.74 nightly? or It can add  the judgment of  this feature?

---

_Comment by @epage on 2024-03-06 12:17_

It supports 1.74 stable. If you want to use it on a nightly of 1.74, it up to you to find a compatible nightly. Alternatively, you can downgrade.

---

_Comment by @jokemanfire on 2024-03-07 01:33_

Understood, I will change it to 1.74 stable.  thanks

---

_Closed by @jokemanfire on 2024-03-07 01:33_

---
