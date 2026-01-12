```yaml
number: 4733
title: "`clippy::almost_swapped` check failing on latest nightly"
type: issue
state: closed
author: rkrasiuk
labels:
  - C-bug
assignees: []
created_at: 2023-02-27T09:26:19Z
updated_at: 2023-03-13T18:27:51Z
url: https://github.com/clap-rs/clap/issues/4733
synced_at: 2026-01-12T16:14:16Z
```

# `clippy::almost_swapped` check failing on latest nightly

---

_@rkrasiuk_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0-nightly (d962ea578 2023-02-26)

### Clap Version

4.1.4

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct SomeArgs {
    name: Vec<String>,
}
```


### Steps to reproduce the bug with the above code

`cargo clippy`

### Actual Behaviour

```sh
$ cargo clippy 
    Checking `<project>` v0.1.0 (`<path>`)
error: this looks like you are trying to swap `arg` and `name: Vec<String>`
 --> src/main.rs:5:5
  |
5 |     name: Vec<String>,
  |     ^^^^^^^^^^^^^^^^^ help: try: `std::mem::swap(&mut arg, &mut name: Vec<String>)`
  |
  = note: or maybe you should use `std::mem::replace`?
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#almost_swapped
note: the lint level is defined here
 --> src/main.rs:3:10
  |
3 | #[derive(Parser, Debug)]
  |          ^^^^^^
  = note: `#[deny(clippy::almost_swapped)]` implied by `#[deny(clippy::correctness)]`
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)

error: could not compile `<project>` due to previous error
```

### Expected Behaviour

No clippy errors

### Additional Context

```sh
$ cargo expand
```

```rust
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
use clap::Parser;
struct SomeArgs {
    name: Vec<String>,
}
impl clap::Parser for SomeArgs {}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
impl clap::CommandFactory for SomeArgs {
    fn command<'b>() -> clap::Command {
        let __clap_app = clap::Command::new("op-reth");
        <Self as clap::Args>::augment_args(__clap_app)
    }
    fn command_for_update<'b>() -> clap::Command {
        let __clap_app = clap::Command::new("op-reth");
        <Self as clap::Args>::augment_args_for_update(__clap_app)
    }
}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
impl clap::FromArgMatches for SomeArgs {
    fn from_arg_matches(
        __clap_arg_matches: &clap::ArgMatches,
    ) -> ::std::result::Result<Self, clap::Error> {
        Self::from_arg_matches_mut(&mut __clap_arg_matches.clone())
    }
    fn from_arg_matches_mut(
        __clap_arg_matches: &mut clap::ArgMatches,
    ) -> ::std::result::Result<Self, clap::Error> {
        #![allow(deprecated)]
        let v = SomeArgs {
            name: __clap_arg_matches
                .remove_many::<String>("name")
                .map(|v| v.collect::<Vec<_>>())
                .unwrap_or_else(Vec::new),
        };
        ::std::result::Result::Ok(v)
    }
    fn update_from_arg_matches(
        &mut self,
        __clap_arg_matches: &clap::ArgMatches,
    ) -> ::std::result::Result<(), clap::Error> {
        self.update_from_arg_matches_mut(&mut __clap_arg_matches.clone())
    }
    fn update_from_arg_matches_mut(
        &mut self,
        __clap_arg_matches: &mut clap::ArgMatches,
    ) -> ::std::result::Result<(), clap::Error> {
        #![allow(deprecated)]
        if __clap_arg_matches.contains_id("name") {
            #[allow(non_snake_case)]
            let name = &mut self.name;
            *name = __clap_arg_matches
                .remove_many::<String>("name")
                .map(|v| v.collect::<Vec<_>>())
                .unwrap_or_else(Vec::new);
        }
        ::std::result::Result::Ok(())
    }
}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
impl clap::Args for SomeArgs {
    fn group_id() -> Option<clap::Id> {
        Some(clap::Id::from("SomeArgs"))
    }
    fn augment_args<'b>(__clap_app: clap::Command) -> clap::Command {
        {
            let __clap_app = __clap_app
                .group(
                    clap::ArgGroup::new("SomeArgs")
                        .multiple(true)
                        .args({
                            let members: [clap::Id; 1usize] = [clap::Id::from("name")];
                            members
                        }),
                );
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("name")
                        .value_name("NAME")
                        .num_args(1..)
                        .value_parser({
                            use ::clap::builder::via_prelude::*;
                            let auto = ::clap::builder::_AutoValueParser::<
                                String,
                            >::new();
                            (&&&&&&auto).value_parser()
                        })
                        .action(clap::ArgAction::Append);
                    let arg = arg;
                    let arg = arg;
                    arg
                });
            __clap_app
        }
    }
    fn augment_args_for_update<'b>(__clap_app: clap::Command) -> clap::Command {
        {
            let __clap_app = __clap_app
                .group(
                    clap::ArgGroup::new("SomeArgs")
                        .multiple(true)
                        .args({
                            let members: [clap::Id; 1usize] = [clap::Id::from("name")];
                            members
                        }),
                );
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("name")
                        .value_name("NAME")
                        .num_args(1..)
                        .value_parser({
                            use ::clap::builder::via_prelude::*;
                            let auto = ::clap::builder::_AutoValueParser::<
                                String,
                            >::new();
                            (&&&&&&auto).value_parser()
                        })
                        .action(clap::ArgAction::Append);
                    let arg = arg;
                    let arg = arg.required(false);
                    arg
                });
            __clap_app
        }
    }
}
#[automatically_derived]
impl ::core::fmt::Debug for SomeArgs {
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        ::core::fmt::Formatter::debug_struct_field1_finish(
            f,
            "SomeArgs",
            "name",
            &&self.name,
        )
    }
}
```

### Debug Output

_No response_

---

_Label `C-bug` added by @rkrasiuk on 2023-02-27 09:26_

---

_Referenced in [paradigmxyz/reth#1572](../../paradigmxyz/reth/issues/1572.md) on 2023-02-27 09:28_

---

_Renamed from "`clippy::correctness` check failing on latest nightly" to "`clippy::almost_swapped` check failing on latest nightly" by @rkrasiuk on 2023-02-27 09:32_

---

_Referenced in [clap-rs/clap#4735](../../clap-rs/clap/pulls/4735.md) on 2023-02-27 09:46_

---

_Comment by @Ltrlg on 2023-02-27 10:26_

There is a similar error with subcommands:

```rust
#[derive(clap::Subcommand)]
enum Command {
    X
}
```

results in:

```
error: this looks like you are trying to swap `__clap_subcommand` and `clap::Subcommand`
 --> src/lib.rs:3:10
  |
3 | #[derive(clap::Subcommand)]
  |          ^^^^^^^^^^^^^^^^
  |
  = note: or maybe you should use `std::mem::replace`?
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#almost_swapped
note: the lint level is defined here
 --> src/lib.rs:3:10
  |
3 | #[derive(clap::Subcommand)]
  |          ^^^^^^^^^^^^^^^^
  = note: `#[deny(clippy::almost_swapped)]` implied by `#[deny(clippy::correctness)]`
  = note: this error originates in the derive macro `clap::Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)
```

The relevant part of the expansion seems to be:

```rust
impl clap::Subcommand for Command {
    fn augment_subcommands<'b>(__clap_app: clap::Command) -> clap::Command {
        ;
        let __clap_app = __clap_app;
        let __clap_app =
            __clap_app.subcommand({
                    let __clap_subcommand = clap::Command::new("x");
                    let __clap_subcommand = __clap_subcommand;
                    let __clap_subcommand = __clap_subcommand;
                    __clap_subcommand
                });
        ;
        __clap_app
    }
    fn augment_subcommands_for_update<'b>(__clap_app: clap::Command)
        -> clap::Command {
        ;
        let __clap_app = __clap_app;
        let __clap_app =
            __clap_app.subcommand({
                    let __clap_subcommand = clap::Command::new("x");
                    let __clap_subcommand = __clap_subcommand;
                    let __clap_subcommand = __clap_subcommand;
                    __clap_subcommand
                });
        ;
        __clap_app
    }
    fn has_subcommand(__clap_name: &str) -> bool {
        if "x" == __clap_name { return true }
        false
    }
}
```

---

_Referenced in [ruffle-rs/ruffle#9756](../../ruffle-rs/ruffle/pulls/9756.md) on 2023-02-27 10:29_

---

_Comment by @rkrasiuk on 2023-02-27 10:47_

@Ltrlg ty, this should be fixed in https://github.com/clap-rs/clap/pull/4735/commits/c1caf462ce40fc541066dfb2a9cccb586777a757 as well

---

_Referenced in [candy-lang/candy#353](../../candy-lang/candy/pulls/353.md) on 2023-02-27 11:09_

---

_Referenced in [0xPolygonZero/evm-tests#15](../../0xPolygonZero/evm-tests/pulls/15.md) on 2023-02-27 18:08_

---

_Closed by @epage on 2023-02-27 19:52_

---

_Comment by @rkrasiuk on 2023-02-27 21:02_

@epage seems like we need to reopen this, because `#[deny(clippy::correctness)]` takes precedence over `#[allow(clippy::almost_swapped)]`

---

_Referenced in [rust-lang/rust-clippy#10421](../../rust-lang/rust-clippy/issues/10421.md) on 2023-02-28 04:17_

---

_Comment by @MingweiSamuel on 2023-02-28 04:18_

Made a clippy issue: https://github.com/rust-lang/rust-clippy/issues/10421

Not clear to me that this should trigger clippy at all. At the very least the error message is wrong

---

_Referenced in [hydro-project/hydro#416](../../hydro-project/hydro/issues/416.md) on 2023-02-28 04:22_

---

_Comment by @MingweiSamuel on 2023-02-28 04:27_

Seems unnecessary to have `#[deny(clippy::correctness)]` in the first place (added in https://github.com/clap-rs/clap/commit/bfc850e99a26747de847fcc15b4786836e8dc4c0 )

Macros aren't really where you want clippy to be at its strictest. And really shouldn't be having clippy to catch bugs in macro code from user usage..

---

_Referenced in [clap-rs/clap#4739](../../clap-rs/clap/pulls/4739.md) on 2023-02-28 05:27_

---

_Referenced in [clap-rs/clap#4740](../../clap-rs/clap/pulls/4740.md) on 2023-02-28 06:28_

---

_Referenced in [bpfman/bpfman#347](../../bpfman/bpfman/pulls/347.md) on 2023-02-28 16:20_

---

_Referenced in [ralexstokes/mev-rs#89](../../ralexstokes/mev-rs/issues/89.md) on 2023-02-28 17:19_

---

_Referenced in [bpfman/bpfman#348](../../bpfman/bpfman/issues/348.md) on 2023-02-28 18:02_

---

_Referenced in [deislabs/spiderlightning#342](../../deislabs/spiderlightning/pulls/342.md) on 2023-03-02 16:56_

---

_Referenced in [svix/svix-webhooks#866](../../svix/svix-webhooks/pulls/866.md) on 2023-03-07 22:03_

---

_Referenced in [libp2p/rust-libp2p#3509](../../libp2p/rust-libp2p/pulls/3509.md) on 2023-03-08 08:12_

---

_Referenced in [libp2p/rust-libp2p#3569](../../libp2p/rust-libp2p/pulls/3569.md) on 2023-03-08 08:24_

---

_Comment by @blyxyas on 2023-03-13 18:27_

I've made a [PR to Clippy](https://github.com/rust-lang/rust-clippy/pull/10499), in a few days it should be fixed in Clippy itself (even without changing Clap's version).

---

_Referenced in [clap-rs/clap#4849](../../clap-rs/clap/issues/4849.md) on 2023-04-20 16:44_

---

_Referenced in [rust-lang/rust-clippy#10685](../../rust-lang/rust-clippy/issues/10685.md) on 2023-04-21 11:36_

---

_Referenced in [stellar/rs-stellar-xdr#247](../../stellar/rs-stellar-xdr/pulls/247.md) on 2023-04-21 18:53_

---

_Referenced in [ordinals/ord#2037](../../ordinals/ord/pulls/2037.md) on 2023-04-22 23:22_

---

_Referenced in [FuelLabs/forc-wallet#110](../../FuelLabs/forc-wallet/pulls/110.md) on 2023-04-24 11:32_

---

_Referenced in [stackabletech/stackablectl#253](../../stackabletech/stackablectl/pulls/253.md) on 2023-04-25 08:58_

---

_Referenced in [oxidecomputer/dice-util#28](../../oxidecomputer/dice-util/issues/28.md) on 2023-04-26 18:57_

---

_Referenced in [oxidecomputer/dice-util#29](../../oxidecomputer/dice-util/pulls/29.md) on 2023-04-26 19:02_

---

_Referenced in [solana-foundation/anchor#2474](../../solana-foundation/anchor/pulls/2474.md) on 2023-04-27 08:07_

---

_Referenced in [tembo/tembo#290](../../tembo/tembo/pulls/290.md) on 2023-04-27 14:43_

---

_Referenced in [cocogitto/cocogitto#280](../../cocogitto/cocogitto/pulls/280.md) on 2023-05-02 08:09_

---

_Referenced in [cargo-checkmate/cargo-checkmate#73](../../cargo-checkmate/cargo-checkmate/pulls/73.md) on 2023-05-09 16:03_

---

_Referenced in [MercuryTechnologies/nix-your-shell#27](../../MercuryTechnologies/nix-your-shell/pulls/27.md) on 2023-06-01 16:35_

---
