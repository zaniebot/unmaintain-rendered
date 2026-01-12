```yaml
number: 4766
title: docs.rs/clap/4.1.9 is broken, missing Command struct documentation
type: issue
state: closed
author: AndASM
labels:
  - C-bug
assignees: []
created_at: 2023-03-17T00:24:12Z
updated_at: 2023-03-24T17:39:40Z
url: https://github.com/clap-rs/clap/issues/4766
synced_at: 2026-01-12T16:14:16Z
```

# docs.rs/clap/4.1.9 is broken, missing Command struct documentation

---

_@AndASM_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

any

### Clap Version

4.1.9

### Minimal reproducible code

Open https://docs.rs/clap/4.1.9/clap/all.html


### Steps to reproduce the bug with the above code

1. Open https://docs.rs/clap/4.1.9/clap/all.html
2. Look for the Command struct
3. Look at https://docs.rs/clap/4.1.8/clap/struct.Command.html for comparison

### Actual Behaviour

The documentation is missing the core structures and methods for the library's functionality.

### Expected Behaviour

The core methods of the library providing it's basic features are documented.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @AndASM on 2023-03-17 00:24_

---

_Comment by @epage on 2023-03-17 00:42_

Nothing related to this change: https://diff.rs/clap/4.1.8/4.1.9/Cargo.toml

I'm guessing this is a rustdoc bug seeing as docs.rs builds using a nightly
- ✅ rustc 1.70.0-nightly (900c35403 2023-03-08)
- ❗ rustc 1.70.0-nightly (ab654863c 2023-03-15)
- ❗ +nightly-2023-03-11
- ✅ +nightly-2023-03-10

Looks like it broke on the 11th

---

_Comment by @AndASM on 2023-03-17 00:52_

Do you know who, how, or where to report that or determine that? I'm newer to Rust and early enough in the learning curve that I get lost and overloaded a bit easy.

---

_Comment by @epage on 2023-03-17 00:55_

I've reached out to the rustdoc team to see if this was already known (I suspect so).

My first step for this was to look through [rust-lang/rust](https://github.com/rust-lang/rust/) issues and PRs in case it was previously reported (at least I think rustdoc lives there)

---

_Comment by @epage on 2023-03-17 13:21_

Upstream issue: https://github.com/rust-lang/rust/issues/109258

Upstream PR: https://github.com/rust-lang/rust/pull/109259

---

_Referenced in [Aloso/to-html#15](../../Aloso/to-html/pulls/15.md) on 2023-03-18 22:26_

---

_Comment by @epage on 2023-03-24 17:39_

Looks like its fixed now

---

_Closed by @epage on 2023-03-24 17:39_

---
