---
number: 4857
title: Some clap v3.x Subcommand derives cause clippy errors since Rust 1.69.0
type: issue
state: closed
author: hds
labels:
  - C-bug
  - E-easy
  - A-derive
assignees: []
created_at: 2023-04-25T13:35:15Z
updated_at: 2023-06-14T13:37:59Z
url: https://github.com/clap-rs/clap/issues/4857
synced_at: 2026-01-10T01:28:02Z
---

# Some clap v3.x Subcommand derives cause clippy errors since Rust 1.69.0

---

_Issue opened by @hds on 2023-04-25 13:35_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0 (84c898d65 2023-04-16)

### Clap Version

clap 3.2.23, clap_derive 3.2.18

### Minimal reproducible code

```rust
#[derive(clap::Subcommand)]
enum Command {
    One,
}

fn main() {}
```


### Steps to reproduce the bug with the above code

Run clippy with defaults:

```sh
cargo clippy
```

### Actual Behaviour

This results in the following error in clippy.

```
error: this looks like you are trying to swap `__clap_subcommand` and `clap::Subcommand`
 --> src/main.rs:1:10
  |
1 | #[derive(clap::Subcommand)]
  |          ^^^^^^^^^^^^^^^^
  |
  = note: or maybe you should use `std::mem::replace`?
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#almost_swapped
note: the lint level is defined here
 --> src/main.rs:1:10
  |
1 | #[derive(clap::Subcommand)]
  |          ^^^^^^^^^^^^^^^^
  = note: `#[deny(clippy::almost_swapped)]` implied by `#[deny(clippy::correctness)]`
  = note: this error originates in the derive macro `clap::Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)

error: could not compile `clap-deny-correctness` due to previous error
```

### Expected Behaviour

The clippy error is problematic because the attribute that specifies `deny` is inside the macro itself. That means that as a user of Clap, I'm not able to override the deny.

Specifically this attribute:
```rust
#[deny(clippy::correctness)]
```

Which is present in 7 specific places in `clap_derive`.

### Additional Context

For Clap v4.x, these attributes were already removed in #4739.

Doing the same in the `v3-master` branch is probably sufficient

### Debug Output

_No response_

---

_Label `C-bug` added by @hds on 2023-04-25 13:35_

---

_Label `E-easy` added by @epage on 2023-04-25 13:41_

---

_Label `A-derive` added by @epage on 2023-04-25 13:41_

---

_Comment by @epage on 2023-04-25 13:41_

I am fine with someone fixing this in v3-master.

---

_Referenced in [clap-rs/clap#4858](../../clap-rs/clap/pulls/4858.md) on 2023-04-25 13:44_

---

_Referenced in [near/nearcore#8937](../../near/nearcore/pulls/8937.md) on 2023-04-25 14:34_

---

_Comment by @guswynn on 2023-04-26 23:40_

@epage Can we `#[allow(almost_swapped)]` in v3 instead of just removing the deny clause? It's quite onerous to allow these warnings, as annotating the struct/enum with `#[allow(clippy::almost_swapped)]` doesn't work, you have to use a `#![allow(clippy::almost_swapped)]` at the module level. If one wants to limit the scope of this allow, they have to introduce a new submodule for all their clap-derived types.

I'm willing to make the pr to do this!

---

_Referenced in [MaterializeInc/materialize#18992](../../MaterializeInc/materialize/pulls/18992.md) on 2023-04-26 23:42_

---

_Comment by @epage on 2023-04-27 00:38_

I will say that the idea is to match clap v4.  We should treat anything in v3 as backports.  I should have looked more closely to confirm that.  I would be open to a PR if it makes things more aligned with v4.  If someone wants something that is not in v4, we should put it in v4 first.

---

_Comment by @hds on 2023-04-27 08:42_

Adding `#![allow(clippy::almost_swapped)]` was already done in v4 (in #4735). I didn't include it in #4858 for the sake of keeping it as small as possible. If @epage is happy for this to also be backported, then I can create the PR.

---

_Comment by @epage on 2023-04-27 14:09_

I would be fine with that

---

_Comment by @guswynn on 2023-04-27 14:40_

Thanks for following up here @hds !

---

_Referenced in [spinframework/spin#1426](../../spinframework/spin/pulls/1426.md) on 2023-04-27 14:55_

---

_Referenced in [clap-rs/clap#4866](../../clap-rs/clap/pulls/4866.md) on 2023-04-27 21:12_

---

_Comment by @hds on 2023-04-27 21:26_

I've backported the v4 change #4735 to `v3-master`, now available in #4866.

The same docs test is failing that failed in the previous PR on that branch.

---

_Comment by @epage on 2023-06-14 13:37_

I believe this is now resolved, so I'm going to close this.  If there is something specific to this issue that still isn't resolved, let us know!

---

_Closed by @epage on 2023-06-14 13:37_

---
