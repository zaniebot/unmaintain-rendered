---
number: 2911
title: Mutable app functions are confusing to users and awkward to work with
type: issue
state: open
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - E-medium
assignees: []
created_at: 2021-10-19T15:33:36Z
updated_at: 2022-09-07T14:41:04Z
url: https://github.com/clap-rs/clap/issues/2911
synced_at: 2026-01-10T01:27:28Z
---

# Mutable app functions are confusing to users and awkward to work with

---

_Issue opened by @epage on 2021-10-19 15:33_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

```rust
let mut app = ...;
app.render_usage();
```


### Steps to reproduce the bug with the above code

Program with the `&mut` API

### Actual Behaviour

Its awkward and exposing of implementation details.  I've seen user confusion akin to "Why does rendering need to modify the app?"

Other uses of this API, like `clap_generate`, need to use the hidden `_build`

### Expected Behaviour

The types are clean and understandable for a user without ambiguous "modify after build"

### Additional Context

I propose we add a `struct AppParser` that derefs to `App`.
- `get_matches` will consume `App` and use `_build` (only build what is needed for parsing)
- `App::build(self) -> AppParser` will call `_build_all`
  - Contains all of the existing `&mut` functions, but without the `mut`

We can implement the additions without breaking behavior; we just add deprecations.  Then on the next breaking release, we remove the deprecated behavior.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-19 15:33_

---

_Label `E: breaking change` added by @epage on 2021-10-19 15:33_

---

_Label `C: app` added by @epage on 2021-10-19 15:33_

---

_Added to milestone `3.0` by @epage on 2021-10-19 15:33_

---

_Assigned to @epage by @epage on 2021-10-19 15:33_

---

_Removed from milestone `3.0` by @epage on 2021-10-19 15:33_

---

_Comment by @pksunkara on 2021-10-19 19:08_

Same issue as `print_*_help` and `write_*_help` methods. I wanted them to be non-mut but couldn't find a way to do it without quickly blowing up the scope of it. IIRC there were some discussions around this when those signatures were implemented which might be useful here.

---

_Comment by @epage on 2021-10-19 19:36_

What are your thoughts on the proposal for solving the `mut` problem?

>  IIRC there were some discussions around this when those signatures were implemented which might be useful here.

At least in my initial search, I found https://github.com/clap-rs/clap/pull/540 which didn't have any useful conversation on the subject.

---

_Comment by @pksunkara on 2021-10-19 19:41_

It would be a good approach to explore definitely. Will have to evaluate it properly when we are finalising the design though.

---

_Comment by @pksunkara on 2021-10-19 19:46_

Heh, weird. Couldn't find the discussion now. Maybe it was on something else that was related? 🤷 

---

_Referenced in [clap-rs/clap#2924](../../clap-rs/clap/issues/2924.md) on 2021-11-05 13:27_

---

_Referenced in [epage/clapng#224](../../epage/clapng/issues/224.md) on 2021-12-06 22:21_

---

_Referenced in [clap-rs/clap#3089](../../clap-rs/clap/issues/3089.md) on 2021-12-08 19:48_

---

_Referenced in [clap-rs/clap#992](../../clap-rs/clap/issues/992.md) on 2021-12-10 18:31_

---

_Referenced in [clap-rs/clap#2218](../../clap-rs/clap/issues/2218.md) on 2021-12-13 15:10_

---

_Added to milestone `4.0` by @epage on 2021-12-13 22:02_

---

_Label `E-medium` added by @epage on 2021-12-13 22:03_

---

_Comment by @epage on 2021-12-14 15:30_

~~We should probably do https://github.com/clap-rs/clap/issues/3089 first.~~
Done

---

_Referenced in [clap-rs/clap#3322](../../clap-rs/clap/pulls/3322.md) on 2022-01-21 14:52_

---

_Referenced in [clap-rs/clap#3221](../../clap-rs/clap/issues/3221.md) on 2022-01-25 15:51_

---

_Referenced in [clap-rs/clap#950](../../clap-rs/clap/issues/950.md) on 2022-02-15 15:48_

---

_Comment by @epage on 2022-02-15 15:49_

#950 needs us to rename the `get_matches` functions.  We should coordinate these changes together to reduce user churn.

---

_Referenced in [clap-rs/clap#3602](../../clap-rs/clap/issues/3602.md) on 2022-04-03 01:10_

---

_Referenced in [clap-rs/clap#3642](../../clap-rs/clap/pulls/3642.md) on 2022-04-19 15:14_

---

_Removed from milestone `4.0` by @epage on 2022-06-01 21:12_

---

_Added to milestone `5.0` by @epage on 2022-06-01 21:12_

---

_Referenced in [clap-rs/clap#4183](../../clap-rs/clap/issues/4183.md) on 2022-09-06 14:08_

---

_Comment by @epage on 2022-09-07 14:41_

A simple way we can handle this is if we add the type
```rust
pub struct Built<T>(T);

impl Built<Command> {
   pub fn get_arguments(&self) -> impl Iterator<Item=Built<Arg>>;

   pub fn get_subcommands(&self) -> impl Iterator<Item=Built<Command>>;
}

impl Built<&'_ Command> {
   pub fn get_arguments(&self) -> impl Iterator<Item=Built<Arg>>;

   pub fn get_subcommands(&self) -> impl Iterator<Item=Built<Command>>;
}

impl<T> AsRef<T> for Built<T>{
    #[inline]
    fn as_ref(&self) -> &T{
        &self.0
    }
}

impl<T> Borrow<T> for Built<T>{
    #[inline]
    fn borrow(&self) -> &T{
        &self.0
    }
}

impl<T> :Deref for Built<T> {
    type Target = T;

    #[inline]
    fn deref(&self) -> &T{
        &self.0
    }
}
```

We would also remove the `get_arguments` getters on `Command` in favor of `get_arguments_mut` to keep clear that `Command` isn't built

This still leaves trying to re-work the parser so it can work off of `mut` and immutable `Command`.

---

_Referenced in [clap-rs/clap#4685](../../clap-rs/clap/issues/4685.md) on 2023-02-01 16:56_

---

_Referenced in [clap-rs/clap#5065](../../clap-rs/clap/issues/5065.md) on 2023-08-17 17:50_

---

_Referenced in [clap-rs/clap#5300](../../clap-rs/clap/issues/5300.md) on 2024-01-12 18:30_

---

_Referenced in [clap-rs/clap#5815](../../clap-rs/clap/issues/5815.md) on 2024-11-13 22:23_

---
