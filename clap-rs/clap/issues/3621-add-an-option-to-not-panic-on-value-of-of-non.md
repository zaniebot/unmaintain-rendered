---
number: 3621
title: "Add an option to not panic on value*of() of non valid arguments"
type: issue
state: closed
author: lu-zero
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-04-10T08:14:09Z
updated_at: 2022-05-18T00:15:55Z
url: https://github.com/clap-rs/clap/issues/3621
synced_at: 2026-01-10T01:27:44Z
---

# Add an option to not panic on value*of() of non valid arguments

---

_Issue opened by @lu-zero on 2022-04-10 08:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap-3.1.8

### Describe your use case

The `cargo` feature exposes is_valid() as workaround but it makes quite cumbersome to deal with code that has similar needs.

### Describe the solution you'd like

Adding a setting to simply return None would spare quite a bit of boilerplate.

### Alternatives, if applicable

Make so the `cargo` feature re-enable the former behavior.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @lu-zero on 2022-04-10 08:14_

---

_Referenced in [lu-zero/cargo-c#255](../../lu-zero/cargo-c/pulls/255.md) on 2022-04-10 08:33_

---

_Comment by @epage on 2022-04-11 12:41_

It'd help if you explained why you don't want arguments validated.

> Make so the cargo feature re-enable the former behavior.

What does the `cargo` feature have to do with this request?

---

_Comment by @lu-zero on 2022-04-11 13:27_

The use case is the same as cargo's, the `cargo` feature is already there to accommodate the needs of cargo.

---

_Comment by @epage on 2022-04-11 15:33_

> The use case is the same as cargo's

Can you go into more detail.  What are you doing, how, and why.  Understanding what people are trying to accomplish with a request is important for shaping features.

>  the cargo feature is already there to accommodate the needs of cargo.

The `cargo` feature is for not for the needs of cargo.  In fact, cargo doesn't enable it.

From the [README](https://github.com/clap-rs/clap#optional-features)

> cargo: Turns on macros that read values from CARGO_* environment variables.

Example of something relying on this feature: https://docs.rs/clap/latest/clap/macro.command.html

---

_Comment by @lu-zero on 2022-04-11 15:43_

> > The use case is the same as cargo's
> 
> Can you go into more detail. What are you doing, how, and why. Understanding what people are trying to accomplish with a request is important for shaping features.

cargo has multiple applets and some codepaths are common and they rely now on `is_valid()` to simulate what previously was simply the use of `value_of()` and managing the None path.

> > the cargo feature is already there to accommodate the needs of cargo.
> 
> The `cargo` feature is for not for the needs of cargo. In fact, cargo doesn't enable it.
> 
> From the [README](https://github.com/clap-rs/clap#optional-features)

Sorry, I meant treat it as the `is_valid*()` set of functions that cargo uses.



---

_Comment by @epage on 2022-04-11 15:57_

> cargo has multiple applets and some codepaths are common and they rely now on is_valid() to simulate what previously was simply the use of value_of() and managing the None path.

Yes, I'm the one who ported cargo to clap3 and added `is_valid` for that purpose.

I'm taking it from your comments that you are depending directly on cargo and are re-using its flag definitions and flag reading?

---

_Comment by @lu-zero on 2022-04-11 15:59_

yes.

---

_Comment by @epage on 2022-04-11 16:01_

Huh, for some reason I created an issue for finding a longer term solution for cargo but I can't find it.  Guess we'll use this for tracking that then.

---

_Label `A-parsing` added by @epage on 2022-04-11 16:02_

---

_Label `S-waiting-on-design` added by @epage on 2022-04-11 16:02_

---

_Comment by @lu-zero on 2022-04-11 16:02_

Thank you :)

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 21:47_

---

_Closed by @epage on 2022-05-18 00:15_

---
