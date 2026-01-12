```yaml
number: 992
title: "Subcommand bin_name on Windows contains \".exe\" in the middle instead of at the end (or not at all)"
type: issue
state: closed
author: Arnavion
labels:
  - C-bug
  - A-help
  - E-easy
  - S-waiting-on-design
assignees: []
created_at: 2017-06-30T18:45:39Z
updated_at: 2022-12-26T18:26:54Z
url: https://github.com/clap-rs/clap/issues/992
synced_at: 2026-01-12T16:14:10Z
```

# Subcommand bin_name on Windows contains ".exe" in the middle instead of at the end (or not at all)

---

_@Arnavion_

### Rust Version

`rustc 1.20.0-nightly (f590a44ce 2017-06-27)`

### Affected Version of clap

`2.25.0`

### Steps to Reproduce the issue

1. `Cargo.toml`:

    ```toml
    [package]
    name = "foo"
    version = "0.1.0"

    [dependencies]
    clap = "*"
    ```

1. `src/main.rs`:

    ```rust
    #[macro_use]
    extern crate clap;

    fn main() {
        let app = clap_app!(
            @app (app_from_crate!())
            (@subcommand bar => )
        );
        let _matches = app.get_matches();
    }
    ```

1. `cargo run -- bar --help` (This runs `target\debug\foo.exe bar --help`. Note the `.exe`). Alternatively, run `.\target\debug\foo.exe bar --help` in cmd or PS. Both these will set `argv[0]` to something ending with `foo.exe`

    Also, running `.\target\debug\foo bar --help` in PS (note doesn't have `.exe`) will still have `argv[0]` ending with `foo.exe` because PS adds it automatically when spawning the process. cmd does not have this behavior so `.\target\debug\foo bar --help` in cmd will have `argv[0]` ending in `foo`

### Expected Behavior Summary

Output should say `foo-bar` or `foo-bar.exe` as the name above the usage.

### Actual Behavior Summary

Output says `foo.exe-bar` as the name above the usage. Note that the usage itself prints `foo.exe bar` which is fine, just the name printed above the usage is the problem.

```
foo.exe-bar

USAGE:
    foo.exe bar

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

### Debug output

>Compile clap with cargo features `"debug"` such as:

[The `debug` feature does not build on Windows.](https://github.com/kbknapp/clap-rs/blob/e27a6ffb/src/app/parser.rs#L6-L7)

### Workaround

Provide the `bin_name` manually instead of letting clap [parse it from `std::env::args_os()`.](https://github.com/kbknapp/clap-rs/blob/e27a6ffb/src/app/mod.rs#L1610)

```
    let app = clap_app!(
        @app (app_from_crate!()) (bin_name: "foo")
        (@subcommand bar => )
    );
```

---

_Comment by @Arnavion on 2017-06-30 19:01_

Just stripping `.exe` from the end of `argv[0]` if `#[cfg(windows)]` should be fine, I think.

Of course this will lead to the wrong output from `foo.exe --help` if someone names their binary `foo.exe.exe` (it'll strip the `.exe` and print `foo`, which won't work), but that is an unlikely situation.

---

_Label `C: help message` added by @kbknapp on 2017-10-04 02:28_

---

_Label `C: subcommands` added by @kbknapp on 2017-10-04 02:28_

---

_Label `P4: nice to have` added by @kbknapp on 2017-10-04 02:28_

---

_Label `T: bug` added by @kbknapp on 2017-10-04 02:28_

---

_Comment by @kbknapp on 2017-10-04 02:30_

When building the subcommand name, I'd be fine with stripping the `.exe` *only* when we do the "concat with subcommands" titling. This would be a very easy fix if someone wants a quick PR.

[Relevant function to fix](https://github.com/kbknapp/clap-rs/blob/3224e2e1cd4927935f44081c1ca686d95f267790/src/app/help.rs#L614-L634)

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 07:06_

---

_Label `D: easy` added by @pksunkara on 2020-04-09 07:07_

---

_Label `Z: good first issue` added by @pksunkara on 2020-04-09 07:07_

---

_Label `Z: mentored` added by @pksunkara on 2020-04-09 07:07_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-23 07:58_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-23 07:58_

---

_Comment by @retep998 on 2020-05-20 03:49_

To avoid issues with whether the shell included the `.exe` in `argv[0]` you can just get the executable name directly on Windows via `GetModuleFileNameW`.

---

_Comment by @CreepySkeleton on 2020-05-20 07:33_

> 
> 
> To avoid issues with whether the shell included the `.exe` in `argv[0]` you can just get the executable name directly on Windows via `GetModuleFileNameW`.

Are there safe bindings to it?

---

_Comment by @retep998 on 2020-05-20 07:46_

Actually, it turns out `std::env::current_exe()` on Windows already does exactly that.

---

_Comment by @pksunkara on 2020-05-20 14:11_

Will you be sending a PR?

---

_Comment by @CreepySkeleton on 2020-05-20 15:23_

It's actually more complicated than it sounds. 

With `current_exe` in mind, we have two competing sources for the application name:
* The first element of the iterator (if `NoBinaryName` wasn't set)
* `current_exe()`

Which one to choose? Do we just discard the element and use `current_exe`?

---

_Comment by @retep998 on 2020-05-20 15:42_

What are the reasons for using `argv[0]` instead of `current_exe()`?

---

_Comment by @CreepySkeleton on 2020-05-20 15:52_

The iterator `clap` parser gets may or may not be from `env::args()`. It can be a user-provided array with it's own first item which is _supposed_ to be used "as if" it was `argv[0]`. It should probably be prioritized over `current_exe`. 

`get_matches` must use `current_exe` and `get_matches_from` must use `iter.nth(0)`, I think.

---

_Comment by @kbknapp on 2020-05-26 22:33_

> The first element of the iterator (if `NoBinaryName` wasn't set)

The purpose of that setting was to start parsing immediately as arguments, instead of skipping `argv[0]`. i.e. such as a REPL or other prompt where the consumer code is continually passing an `argv` direct to clap that doesn't include the binary.

> which is supposed to be used "as if" it was `argv[0]`

That's the incorrect bit :wink: clap assumes the binary name was set manually when `NoBinaryName` is used, it doesn't re-interpret `argv[0]` as a new binary name.

---

_Comment by @kotx on 2021-07-03 05:42_

Any update on this?

---

_Referenced in [epage/clapng#74](../../epage/clapng/issues/74.md) on 2021-12-06 16:34_

---

_Referenced in [clap-rs/clap#3073](../../clap-rs/clap/issues/3073.md) on 2021-12-08 00:16_

---

_Comment by @epage on 2021-12-08 00:30_

We've also run into this when testing examples, like the former `08_subcommands[.exe-add`
```
$ 08_subcommands help add
08_subcommands[.exe-add 0.1

Kevin K.

Adds files to myapp

USAGE:
    08_subcommands.exe add <input>

ARGS:
    <input>    the file to add

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
```

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:15_

---

_Comment by @epage on 2021-12-09 17:04_

Why would we not just strip `std::env::consts::EXE_SUFFIX` from argv[0] when setting the bin name?  Do we really need `.exe` everywhere else?  If `get_matches_from` is meant to act like `get_matches`, it seems like it should be fine for us to strip it from there to.

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 17:04_

---

_Label `D: easy` removed by @epage on 2021-12-09 17:04_

---

_Label `E-medium` removed by @epage on 2021-12-09 17:04_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-09 17:04_

---

_Label `S-waiting-on-decision` removed by @epage on 2021-12-09 17:05_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 17:05_

---

_Comment by @pksunkara on 2021-12-10 02:02_

> Do we really need .exe everywhere else?

I am under the impression that we do. **bin name** has always been about the actual binary name and I think it's been used in other places with that assumption which is why Kevin suggested a new variable to hold this name in the first place.

>  I'd be fine with stripping the .exe only when we do the "concat with subcommands" titling.

If you were to audit the usage of it and decide that it's okay setting bin name without `.exe`, then I am good with your proposal.



---

_Comment by @epage on 2021-12-10 18:31_

> If you were to audit the usage of it and decide that it's okay setting bin name without .exe, then I am good with your proposal.

Looking around, it looks like its only used for display purposes, like help and error messages.  I'm not seeing anything doing processing based on the name.  We don't expose the `bin_name` from `arg_matches`.  We do mutate the `App` with the bin name from the last parse, so someone could do
```rust
let matches = app.get_matches_mut();
let  bin = app.get_bin_name()
```
Personally, that feels hacky; imo all parse results should be coming from `ArgMatches`.   In considering this, a naiive implementation of #2911 would make it so people would never be able to look up the `bin_name` found from parsing.

---

_Referenced in [clap-rs/clap#3041](../../clap-rs/clap/pulls/3041.md) on 2021-12-10 20:58_

---

_Referenced in [clap-rs/clap#2861](../../clap-rs/clap/issues/2861.md) on 2021-12-13 15:24_

---

_Referenced in [clap-rs/clap#3096](../../clap-rs/clap/issues/3096.md) on 2021-12-14 21:59_

---

_Referenced in [clap-rs/clap#1474](../../clap-rs/clap/issues/1474.md) on 2021-12-14 22:02_

---

_Comment by @spenserblack on 2022-01-03 21:30_

> Do we really need `.exe` everywhere else?

Are you asking if `.exe` should be removed *everywhere*?
I think it's slightly useful in the usage section, since it's possible for command prompt to prioritize `my-app.bat` over `my-app.exe`.

---

_Removed from milestone `3.x` by @epage on 2022-02-02 19:53_

---

_Added to milestone `3.1` by @epage on 2022-02-02 19:53_

---

_Removed from milestone `3.1` by @epage on 2022-02-08 20:44_

---

_Added to milestone `3.2` by @epage on 2022-02-08 20:44_

---

_Referenced in [clap-rs/clap#3515](../../clap-rs/clap/pulls/3515.md) on 2022-03-07 20:51_

---

_Referenced in [rust-lang/cargo#10561](../../rust-lang/cargo/issues/10561.md) on 2022-04-13 14:22_

---

_Referenced in [clap-rs/clap#3680](../../clap-rs/clap/pulls/3680.md) on 2022-05-03 19:35_

---

_Removed from milestone `3.2` by @epage on 2022-05-05 01:14_

---

_Added to milestone `4.0` by @epage on 2022-05-05 01:14_

---

_Comment by @epage on 2022-05-05 01:14_

We're close enough to when we can start working on 4.0, I'm going to play it safe and push it off to then

---

_Referenced in [clap-rs/clap#3693](../../clap-rs/clap/pulls/3693.md) on 2022-05-05 01:25_

---

_Closed by @epage on 2022-05-05 17:01_

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-27 02:18_

---

_Comment by @dbsxdbsx on 2022-12-26 02:27_

> Why would we not just strip `std::env::consts::EXE_SUFFIX` from argv[0] when setting the bin name? Do we really need `.exe` everywhere else? If `get_matches_from` is meant to act like `get_matches`, it seems like it should be fine for us to strip it from there to.


(I am using clap 3.2.14)
I wonder how to eliminate the `exe` suffix output after running command on windows when using clap under `derive`.
Currently, 
with cargo.toml:
```toml
[[bin]]
name = "app_name"
path = "src/main.rs"
```
and a struct using clap:
```rust
#[derive(Parser, Debug, PartialEq, Eq, Deserialize, Default)]
#[clap(version, setting(clap::AppSettings::DeriveDisplayOrder))]
pub struct MyStruct {
...
}
```
after running `cargo run --manifest-path [project_path]/Cargo.toml -- --help > book/src/help.txt
`
the `USAGE` section of help.txt is like:
```log
USAGE:
    app_name.exe [OPTIONS] ...
```
I do can eliminate the `exe` by changing the derivative to :
```rust
#[clap(bin_name = "app_name", version, setting(clap::AppSettings::DeriveDisplayOrder))]
```
But is there a way without changing the derivative macro?  Meanwhile, I also wonder how to output like so:
```log
USAGE:
    app_name[EXE] [OPTIONS] ...
```
with `[EXE]` instead of `.exe` whatever platform I am running.

---

_Comment by @epage on 2022-12-26 18:26_

Specifying attributes is how you customize clap via the derive and setting `bin_name` is exactly the way to resolve this.

`[EXE]` is in our documentation due to our [testing framework](https://docs.rs/trycmd/latest/trycmd/) for our docs.

---

_Referenced in [trunk-rs/trunk#556](../../trunk-rs/trunk/issues/556.md) on 2023-06-16 16:35_

---
