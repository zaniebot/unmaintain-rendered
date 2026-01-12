```yaml
number: 714
title: zsh completion fails when subcommands have aliases
type: issue
state: closed
author: nicompte
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2016-10-28T06:53:11Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/714
synced_at: 2026-01-12T16:14:09Z
```

# zsh completion fails when subcommands have aliases

---

_@nicompte_

I'm trying to generate the zsh completion in my `build.rs` file, using clap `2.16.2`:

``` rust
extern crate clap;
use clap::Shell;
include!("src/cli/mod.rs");

fn main() {
    let mut app = build();
    app.gen_completions("fcy", Shell::Fish, env!("OUT_DIR"));
    app.gen_completions("fcy", Shell::Bash, env!("OUT_DIR"));
    app.gen_completions("fcy", Shell::Zsh, env!("OUT_DIR"));
}
```

But the last line makes the build fail (bash and fish work fine):

``` shell
➜ ~ rustc -V
rustc 1.14.0-nightly (c59cb71d9 2016-10-26)
➜ ~ cargo -V
cargo 0.13.0-nightly (806e3c3 2016-10-26)
➜ ~ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    
Release:        15.10
Codename:       wily
```

``` shell
➜  ~ cargo build
error: failed to run custom build command for `fcy v0.0.4 (file:///home/nicompte/Repository/fcy2)`
process didn't exit successfully: `/home/nicompte/Repository/fcy2/target/debug/build/fcy-f49762e438fa7f28/build-script-build` (exit code: 101)
--- stderr
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', ../src/libcore/option.rs:705
stack backtrace:
   1:     0x55f781151248 - std::sys::backtrace::tracing::imp::write::h22f199c1dbb72ba2
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:42
   2:     0x55f78115478f - std::panicking::default_hook::{{closure}}::h9a389c462b6a22dd
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:247
   3:     0x55f7811536c6 - std::panicking::default_hook::h852b4223c1c00c59
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:263
   4:     0x55f781153cb7 - std::panicking::rust_panic_with_hook::hcd9d05f53fa0dafc
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:451
   5:     0x55f781153b44 - std::panicking::begin_panic::hf6c488cee66e7f17
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:413
   6:     0x55f781153a69 - std::panicking::begin_panic_fmt::hb0a7126ee57cdd27
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:397
   7:     0x55f7811539f7 - rust_begin_unwind
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:373
   8:     0x55f7811896bd - core::panicking::panic_fmt::h9af671b78898cdba
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libcore/panicking.rs:69
   9:     0x55f78118972d - core::option::expect_failed::hafc66854f3459293
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libcore/option.rs:705
  10:     0x55f7811085c3 - <core::option::Option<T>>::expect::he1fa75a7269dfe51
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libcore/option.rs:293
  11:     0x55f7811448c7 - clap::completions::zsh::parser_of::h63d4971053837895
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:230
  12:     0x55f7811441e4 - clap::completions::zsh::get_subcommands_of::h6ccc5a8cc84ec3fe
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:202
  13:     0x55f7811442c7 - clap::completions::zsh::get_subcommands_of::h6ccc5a8cc84ec3fe
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:206
  14:     0x55f781142b22 - clap::completions::zsh::ZshGen::generate_to::hd24ded8070bf9d5e
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:28
  15:     0x55f781147081 - clap::completions::ComplGen::generate::h68568c315fd58ac4
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/mod.rs:33
  16:     0x55f7811341e9 - clap::app::parser::Parser::gen_completions_to::ha9762178a111456c
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/app/parser.rs:112
  17:     0x55f781134811 - clap::app::parser::Parser::gen_completions::hf325b7a19d4d643c
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/app/parser.rs:132
  18:     0x55f7810f2a01 - clap::app::App::gen_completions::h413df50734488193
                        at /home/nicompte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/app/mod.rs:1128
  19:     0x55f7810f9337 - build_script_build::main::h42d67b06b82522fd
                        at /home/nicompte/Repository/fcy2/build.rs:11
  20:     0x55f78115c26a - __rust_maybe_catch_panic
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libpanic_unwind/lib.rs:97
  21:     0x55f781152f1a - std::rt::lang_start::h14cbded5fe3cd915
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panicking.rs:332
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/panic.rs:311
                        at /buildslave/rust-buildbot/slave/nightly-dist-rustc-linux/build/obj/../src/libstd/rt.rs:57
  22:     0x55f7810fc2a3 - main
  23:     0x7fb84ee5fabf - __libc_start_main
  24:     0x55f7810f0e18 - _start
  25:                0x0 - <unknown>
```

And with the `debug` feature, if that's the way to get more information:

```
error[E0382]: use of moved value: `s`
   --> /home/nbarbotte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:311:38
    |
310 |             ret.push(s);
    |                      - value moved here
311 |             debugln!("Wrote...{}", &*s);
    |             -------------------------^--
    |             |                        |
    |             |                        value used here after move
    |             in this macro invocation
    |
    = note: move occurs because `s` has type `std::string::String`, which does not implement the `Copy` trait

error[E0382]: use of moved value: `l`
   --> /home/nbarbotte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:322:38
    |
321 |             ret.push(l);
    |                      - value moved here
322 |             debugln!("Wrote...{}", &*l);
    |             -------------------------^--
    |             |                        |
    |             |                        value used here after move
    |             in this macro invocation
    |
    = note: move occurs because `l` has type `std::string::String`, which does not implement the `Copy` trait

error[E0382]: use of moved value: `s`
   --> /home/nbarbotte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:347:38
    |
346 |             ret.push(s);
    |                      - value moved here
347 |             debugln!("Wrote...{}", &*s);
    |             -------------------------^--
    |             |                        |
    |             |                        value used here after move
    |             in this macro invocation
    |
    = note: move occurs because `s` has type `std::string::String`, which does not implement the `Copy` trait

error[E0382]: use of moved value: `l`
   --> /home/nbarbotte/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.16.2/src/completions/zsh.rs:358:38
    |
357 |             ret.push(l);
    |                      - value moved here
358 |             debugln!("Wrote...{}", &*l);
    |             -------------------------^--
    |             |                        |
    |             |                        value used here after move
    |             in this macro invocation
    |
    = note: move occurs because `l` has type `std::string::String`, which does not implement the `Copy` trait

error: aborting due to 4 previous errors
```

Any idea of how I could fix it?


---

_Comment by @kbknapp on 2016-10-29 15:11_

Thanks for posting all these details! 

The `debug` feature appears to be having an issue of it's own :stuck_out_tongue_winking_eye: I just fixed this and pushed it to the master branch.

Could you do me a quick favor and change your `Cargo.toml` to:

```
clap = {git = "https://github.com/kbknapp/clap-rs.git", features = ["debug"]}
```

Then re-run it and post the debug output? This should be an easy fix!


---

_Label `T: bug` added by @kbknapp on 2016-10-29 15:12_

---

_Label `P2: need to have` added by @kbknapp on 2016-10-29 15:12_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-29 15:12_

---

_Label `C: completion gen` added by @kbknapp on 2016-10-29 15:12_

---

_Comment by @nicompte on 2016-10-29 15:41_

Thank you for your answer!

Here it is, on macOs this time. It is quite long though :)

[debug.txt](https://github.com/kbknapp/clap-rs/files/559999/debug.txt)


---

_Comment by @kbknapp on 2016-10-29 18:51_

@nicompte Thanks!

It looks like it's failing to locate the subcommand `logs` for some reason. Do you have a link to the source, so I can see how you declared that subcommand?


---

_Comment by @nicompte on 2016-10-30 09:17_

Unfortunately this project is not open source, I can show you how it's constructed though. 

The idea is that the `logs` subcommand is used at several places and is the subcommand of other subcommands, so you can use `fcy api logs` and `fcy front logs`. Both the `api` and `front` use the same function to get their `logs` subcommand.

But their is no `fcy logs`, it's always under a subcommand.

(probably not compiling code)

``` rust
// log subcommand builder
pub fn logs_build() -> App<'static, 'static> {
    SubCommand::with_name(LOG)
        .about("log")
        .alias(LOGS)
        .alias(TAIL)
       // other options
}

// the two subcommands that use logs, like `fcy api logs`
pub fn build_api() -> App<'static, 'static> {
    SubCommand::with_name(API)
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .about("api")
        .subcommand(logs_build())
       // other subcommands
}
pub fn build_front() -> App<'static, 'static> {
    SubCommand::with_name(FRONT)
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .about("front")
        .subcommand(logs_build())
       // other subcommands
}

// cli creation
App::new("fcy")
        .setting(AppSettings::DeriveDisplayOrder)
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .global_setting(AppSettings::ColoredHelp)
        .global_setting(AppSettings::VersionlessSubcommands)
        .version(env!("CARGO_PKG_VERSION"))
        .author(env!("CARGO_PKG_AUTHORS"))
        .about(env!("CARGO_PKG_DESCRIPTION"))
        .version_short("v")

        .subcommand(build_api())
        .subcommand(build_front())
        // other subcommands
```

Hope that helps, I'll try removing/adding some parts of my code to see when/where it fails.


---

_Comment by @nicompte on 2016-10-30 09:22_

Ok it went faster than I expected, the problem seems to be the aliases of the `logs` subcommand, it works when I remove them.


---

_Comment by @kbknapp on 2016-10-30 22:28_

Oh wow, thanks for the detailed look! Ok, so then it may be aliases that's the actual bug. Let me do some testing on my local machine and see if I can either reproduce the issue, or come up with a test case to get this fixed for you.


---

_Comment by @kbknapp on 2016-10-30 22:39_

Alright I've reproduced the issue, and it is in fact due to aliases. Once I have the fix, I'll post back here.


---

_Renamed from "zsh completion fails" to "zsh completion fails when subcommands have aliases" by @kbknapp on 2016-10-30 22:39_

---

_Label `P1: urgent` added by @kbknapp on 2016-10-30 22:40_

---

_Label `C: subcommands` added by @kbknapp on 2016-10-30 22:40_

---

_Label `P2: need to have` removed by @kbknapp on 2016-10-30 22:40_

---

_Added to milestone `2.16.4` by @kbknapp on 2016-10-30 22:40_

---

_Closed by @homu on 2016-10-31 12:11_

---

_Comment by @nicompte on 2016-10-31 14:12_

@kbknapp: it works fine on the latest version, thanks for your quick fix and all your work on clap 👍 


---
