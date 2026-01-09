---
number: 4279
title: "Flatten structs causes \"Argument group name must be unique\" if has same name"
type: issue
state: closed
author: repi
labels:
  - C-bug
  - A-derive
  - S-blocked
assignees: []
created_at: 2022-09-28T20:12:18Z
updated_at: 2022-09-30T14:55:16Z
url: https://github.com/clap-rs/clap/issues/4279
synced_at: 2026-01-07T13:12:20-06:00
---

# Flatten structs causes "Argument group name must be unique" if has same name

---

_Issue opened by @repi on 2022-09-28 20:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.1

### Minimal reproducible code

```rust
#[derive(clap::Parser, Debug)]
struct Config {
    #[arg(long)]
    pub value1: usize,

    #[clap(flatten)]
    pub config: sub::Config,
}

mod sub {

    #[derive(clap::Parser, Debug)]
    pub struct Config {
        #[arg(long)]
        pub value2: usize,
    }
}

fn main() {
    use clap::Parser;
    let config = Config::parse();

    println!("Hello: {config:?}");
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The above causes panic on:

```
Argument group name must be unique

        'Config' is already in use', /home/repi/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.1/src/builder/debug_asserts.rs:284:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

due to the two `Config` structs having the same name. Maybe caused internally by these being placed in separate argument groups but with the same argument group names instead of being merged together to a single argument group with then name "Config"?

The code 

### Expected Behaviour

Would want this to work out of the box without having to rename the types

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="clap-tests"
[      clap::builder::command]  Command::_propagate:clap-tests
[      clap::builder::command]  Command::_check_help_and_version:clap-tests expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:clap-tests
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:value1
[clap::builder::debug_asserts]  Arg::_debug_asserts:value2
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
thread 'main' panicked at 'Command clap-tests: Argument group name must be unique

        'Config' is already in use', /home/repi/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.1/src/builder/debug_asserts.rs:284:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `C-bug` added by @repi on 2022-09-28 20:12_

---

_Referenced in [EmbarkStudios/rust-ecosystem#36](../../EmbarkStudios/rust-ecosystem/issues/36.md) on 2022-09-28 20:12_

---

_Referenced in [alexheretic/ab-av1#52](../../alexheretic/ab-av1/pulls/52.md) on 2022-09-28 21:31_

---

_Label `A-derive` added by @epage on 2022-09-28 22:13_

---

_Label `S-blocked` added by @epage on 2022-09-28 22:13_

---

_Comment by @epage on 2022-09-28 22:16_

This group was added so we could implement
- https://github.com/clap-rs/clap/issues/3165
- https://github.com/clap-rs/clap/issues/4211
- https://github.com/clap-rs/clap/issues/2621

When we implement https://github.com/clap-rs/clap/issues/3165, we should allow people to rename the implicit group which would be a work around for this that is less disruptive than renaming a type.

As for making this work automatically, I do not have any thoughts / plans for that.

---

_Comment by @repi on 2022-09-28 22:18_

Thanks. So to confirm, the only current solution is to rename the struct in the code so one doesn't have 2 or more structs with the same name if using flattening?

---

_Comment by @epage on 2022-09-28 22:20_

For now, yes.  If you  want, you can wait to upgrade until we implement #3165 to have another workaround.

---

_Comment by @ilslv on 2022-09-30 11:56_

While [upgrading to `0.4` in `cucumber` crate](https://github.com/cucumber-rs/cucumber/pull/230) we've stumbled into this issue, but just renaming structs doesn't work for our use case. 

In short, we use `Compose` generic struct like:

```rust
#[derive(Args, Debug)]
pub struct Compose<L: Args, R: Args> {
    #[clap(flatten)]
    pub left: L,
    #[clap(flatten)]
    pub right: R,
}
```

This `Compose` struct is used to fuse CLIs of different components (some of them may be user-defined). And most of those components doesn't add any new cli flags, so they use `Empty`:

```rust
#[derive(Args, Clone, Copy, Debug)]
pub struct Empty;
```

So we quickly get [`Empty is already in use` panic](https://github.com/cucumber-rs/cucumber/actions/runs/3158414239/jobs/5140421395#step:4:448). And I don't think that https://github.com/clap-rs/clap/issues/3165 is going to help solve this problem. 

---

_Referenced in [cucumber-rs/cucumber#230](../../cucumber-rs/cucumber/pulls/230.md) on 2022-09-30 11:57_

---

_Comment by @tyranron on 2022-09-30 12:31_

Hmmm... what if?
1. Identify a group not by a type name, but by a `TypeId`, or at least canonical name like `::crate::module::Type`? This will auto-resolve any group naming collisions.
2. Regarding the `Empty` case, just allow collisions if a group has no args, because there can be no real collisions in this edge case. Or provide a special empty type in a `clap` crate for such purposes with the reserved group name, so the `clap` crate is aware of it.

---

_Comment by @epage on 2022-09-30 14:04_

@ilslv 
> In short, we use Compose generic struct like:

Thanks for sharing this use case!  Instead of prioritizing other group work, I think i'll focus on `#[group(skip)]` to disable it.

@tyranron 
> Identify a group not by a type name, but by a TypeId, or at least canonical name like ::crate::module::Type? This will auto-resolve any group naming collisions.

Users are also expected to be able to identify and use the group name which means it needs to be easily discoverable.  I'd worry that using the `TypeId` would make it too opaque

> Regarding the Empty case, just allow collisions if a group has no args, because there can be no real collisions in this edge case. Or provide a special empty type in a clap crate for such purposes with the reserved group name, so the clap crate is aware of it.

For now, all groups are empty as I wanted to reserve the names in a breaking release for implementing #3165.  Even after that, while `Empty` would be empty, `Compose` would not and cucumber wouldn't be able to use multiple `Compose`s.

---

_Referenced in [clap-rs/clap#4301](../../clap-rs/clap/pulls/4301.md) on 2022-09-30 14:16_

---

_Referenced in [clap-rs/clap#3165](../../clap-rs/clap/issues/3165.md) on 2022-09-30 14:18_

---

_Closed by @epage on 2022-09-30 14:30_

---

_Comment by @epage on 2022-09-30 14:32_

4.0.6 is released with `#[group(skip)]` support

---

_Comment by @repi on 2022-09-30 14:55_

big thanks! that solved our last remaining issue with clap v4

---

_Referenced in [clap-rs/clap#4307](../../clap-rs/clap/pulls/4307.md) on 2022-09-30 17:57_

---

_Referenced in [autonomys/subspace#1215](../../autonomys/subspace/issues/1215.md) on 2023-03-03 23:26_

---

_Referenced in [coreos/rpm-ostree#5273](../../coreos/rpm-ostree/pulls/5273.md) on 2025-02-05 15:09_

---
