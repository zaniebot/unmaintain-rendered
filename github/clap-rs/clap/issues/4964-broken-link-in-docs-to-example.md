---
number: 4964
title: Broken link in docs to example
type: issue
state: closed
author: c-git
labels:
  - C-bug
assignees: []
created_at: 2023-06-11T00:47:21Z
updated_at: 2023-06-12T15:45:00Z
url: https://github.com/clap-rs/clap/issues/4964
synced_at: 2026-01-07T13:12:20-06:00
---

# Broken link in docs to example

---

_Issue opened by @c-git on 2023-06-11 00:47_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

N/A - Docs

### Clap Version

4.3.3

### Minimal reproducible code

N/A Just docs

### Steps to reproduce the bug with the above code

Go to the [_derive](https://docs.rs/clap/latest/clap/_derive/index.html) in the docs and scroll all the way to the bottom to the Tips section. It says 

> Proactively check for bad [Command](https://docs.rs/clap/latest/clap/struct.Command.html) configurations by calling [Command::debug_assert](https://docs.rs/clap/latest/clap/struct.Command.html#method.debug_assert) in a test ([example](https://docs.rs/clap/latest/clap/tutorial_derive/05_01_assert.rs))

The last link to the example doesn't work: https://docs.rs/clap/latest/clap/tutorial_derive/05_01_assert.rs

### Actual Behaviour

I get 
![image](https://github.com/clap-rs/clap/assets/43485962/8155b35c-9f9f-4094-b90c-5dae02161b04)


### Expected Behaviour

Maybe it should point to <https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#testing> but I don't know, now sure what it used to show.

### Additional Context

_No response_

### Debug Output

N/A

---

_Label `C-bug` added by @c-git on 2023-06-11 00:47_

---

_Closed by @epage on 2023-06-12 15:45_

---
