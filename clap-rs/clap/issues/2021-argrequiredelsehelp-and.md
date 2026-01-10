---
number: 2021
title: "ArgRequiredElseHelp and SubcommandRequiredElseHelp don't result in clap::ErrorKind::HelpDisplayed"
type: issue
state: closed
author: leighmcculloch
labels:
  - C-bug
assignees: []
created_at: 2020-07-18T07:30:43Z
updated_at: 2020-10-11T08:44:13Z
url: https://github.com/clap-rs/clap/issues/2021
synced_at: 2026-01-10T01:27:11Z
---

# ArgRequiredElseHelp and SubcommandRequiredElseHelp don't result in clap::ErrorKind::HelpDisplayed

---

_Issue opened by @leighmcculloch on 2020-07-18 07:30_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

This is an example with `AppSettings::ArgRequiredElseHelp`. This also occurs with `AppSettings::SubcommandRequiredElseHelp`.

```toml
[package]
name = "fake"

[dependencies]
clap = "3.0.0-beta.1"
```
```rust
use clap::{Arg, App, AppSettings, Error};

fn main() -> Result<(), Error> {
    let cli = App::new("myapp")
        .setting(AppSettings::ArgRequiredElseHelp)
        .arg(
            Arg::new("info")
                .about("Provides more info")
                .short('i')
                .long("info"),
        );
    let matches = match cli.try_get_matches() {
        Ok(matches) => matches,
        Err(e)
            if e.kind == clap::ErrorKind::HelpDisplayed
                || e.kind == clap::ErrorKind::VersionDisplayed
                || e.kind == clap::ErrorKind::MissingArgumentOrSubcommand =>
        {
          println!("{:?}", e.kind);
          return Ok(());
        }
        Err(e) => {
          return Err(e);
        }
    };

    // We can find out whether or not debugging was turned on
    if matches.is_present("info") {
        println!("This is more info");
    }
    return Ok(())
}
```

### Steps to reproduce the issue

1. Run `cargo run`

### Version

* **Rust**: `rustc 1.45.0-nightly (a74d1862d 2020-05-14)`
* **Clap**: `3.0.0-beta.1`

### Actual Behavior Summary

When I use `AppSettings::ArgRequiredElseHelp` or `AppSettings::SubcommandRequiredElseHelp` and do not provide an argument or subcommand the matches call returns an error of kind `clap::ErrorKind::MissingArgumentOrSubcommand` which is not documented or intuitive.

It is not intuitive because the `ElseHelp` suggests the result will match that of if the help is being displayed, which would be the `clap::ErrorKind::HelpDisplayed` kind.

Also, the `clap::ErrorKind::MissingArgumentOrSubcommand` error kind is treated as a failure and does not use stderr, as pointed out by @kinnison in https://github.com/rust-lang/rustup/pull/2427#issuecomment-659923231, which is inconsistent with `clap::ErrorKind::HelpDisplayed`.
https://github.com/clap-rs/clap/blob/7127653b507802e005990b88341164aaf940bc14/src/errors.rs#L387-L392

### Expected Behavior Summary

I think there's a few ways this might be improved.

1. Have this situation result in `clap::ErrorKind::HelpDisplayed`. This is intuitive, although maybe not as flexible.

2. Rename the error kind to `clap::ErrorKind::MissingArgumentOrSubcommandHelpDisplayed` to indicate clearly that this error is being returned to display the help, but to retain the flexibility.

3. Add docs indicating that the existing error is returned like HelpDisplayed and VersionDisplayed, so it's not a surprise when this error shows up.

4. Do 3, plus treat the error not as a failure.

### Additional context

This issue is related to a bug in the latest version of rustup: https://github.com/rust-lang/rustup/issues/2425. There is a fix for that bug in rustup https://github.com/rust-lang/rustup/pull/2427 but the rustup maintainers want to confirm if this is a bug in clap.

In rustup v2 of clap is being used but this issue is relevant to both v2 and v3, and looks like it has existed for about 5 years since the ElseHelp settings were added.

### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_derive_display_order:myapp
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::remove_overrides
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::color_help
[           clap::output::fmt]  is_a_tty: stderr=true
[          clap::output::help]  Help::new
[          clap::output::help]  Help::write_help
[          clap::output::help]  Help::write_default_help
[          clap::output::help]  Help::write_bin_name
[          clap::output::help]  Help::write_version
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage]  Usage::needs_flags_tag
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=
[         clap::output::usage]  groups_for_arg: name=info
[         clap::output::usage]  Usage::needs_flags_tag:iter: [FLAGS] required
[         clap::output::usage]  Usage::create_help_usage: usage=fake [FLAGS]
[          clap::output::help]  Help::write_all_args
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...6
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_args: Current Longest...6
[          clap::output::help]  Help::write_args: New Longest...6
[          clap::output::help]  should_show_arg: use_long=false, arg=version
[          clap::output::help]  Help::write_args: Current Longest...6
[          clap::output::help]  Help::write_args: New Longest...9
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=
[          clap::output::help]  Help::spec_vals: a=--info
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...21
[          clap::output::help]  val: help_width > (width - taken)...18 > (120 - 21)
[          clap::output::help]  Help::val: longest...9
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=7
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=help
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...21
[          clap::output::help]  val: help_width > (width - taken)...23 > (120 - 21)
[          clap::output::help]  Help::val: longest...9
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=7
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=version
[          clap::output::help]  Help::spec_vals: a=--version
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...21
[          clap::output::help]  val: help_width > (width - taken)...26 > (120 - 21)
[          clap::output::help]  Help::val: longest...9
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=4
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
MissingArgumentOrSubcommand

</code>
</pre>
</details>


---

_Label `T: bug` added by @leighmcculloch on 2020-07-18 07:30_

---

_Referenced in [rust-lang/rustup#2427](../../rust-lang/rustup/pulls/2427.md) on 2020-07-18 07:31_

---

_Comment by @CreepySkeleton on 2020-07-18 13:16_

This one is complicated. 

First thing to notice: `HelpDisplayed` is __past tense__. The help has _already_ been printed to stdout/stderr; the `ErrorKind` is just a way to notify the developer about that, not a way to give control over the process. This was quite annoying and counter-intuitive, so we changed this - 3.0 doesn't print anything preemptively, and I will rename the error kinds shortly, see #2006. Contrary, `MissingArgumentOrSubcommand` didn't do anything like that, so I guess keeping it separate from `HelpDisplayed` did make some sense.

Otherwise, I see that `MissingArgumentOrSubcommand` isn't useful. Let's just use the new renamed `HelpDisplayed` and have it use stderr, too.

---

_Comment by @leighmcculloch on 2020-07-18 15:31_

Sounds good. Will this just be in v3 and be a feature change, or should it change in v2 as well?

---

_Comment by @CreepySkeleton on 2020-07-18 15:37_

Changing that in v2 would be breaking (low impact, but still). v3 only.

---

_Added to milestone `3.0` by @pksunkara on 2020-10-09 16:42_

---

_Label `C: errors` added by @pksunkara on 2020-10-09 16:43_

---

_Comment by @pksunkara on 2020-10-10 18:58_

@ldm0 Would you like to take this next?

---

_Comment by @ldm0 on 2020-10-10 19:02_

No problem.

---

_Comment by @ldm0 on 2020-10-11 05:35_

> Let's just use the new renamed HelpDisplayed and have it use stderr, too.

The `HelpDisplayed` is currently named as `DisplayHelp`. Removing `MissingArgumentOrSubcommand ` and having it use stderr wouldn be strange, since calling `app --help` will emits help information in stderr.

Also, the `MissingArgumentOrSubcommand` is already using stderr. I guess after the `HelpDisplayed` renaming(which is already done), there is nothing left to do?

---

_Comment by @pksunkara on 2020-10-11 06:32_

> It is not intuitive because the ElseHelp suggests the result will match that of if the help is being displayed, which would be the clap::ErrorKind::HelpDisplayed kind.

> Also, the clap::ErrorKind::MissingArgumentOrSubcommand error kind is treated as a failure and does not use stderr, as pointed out by @kinnison in rust-lang/rustup#2427 (comment), which is inconsistent with clap::ErrorKind::HelpDisplayed

We need to resolve these 2 issues. Let's create a new error `DisplayHelpOnMissingArgumentOrSubcommand` and return it if `ArgRequiredElseHelp` or `SubcommandRequiredElseHelp` is set and they are triggered. Also, let's output only this error to stdout.

---

_Referenced in [clap-rs/clap#2168](../../clap-rs/clap/pulls/2168.md) on 2020-10-11 07:19_

---

_Closed by @bors[bot] on 2020-10-11 08:44_

---

_Referenced in [clap-rs/clap#2767](../../clap-rs/clap/issues/2767.md) on 2021-10-04 20:01_

---

_Referenced in [clap-rs/clap#2859](../../clap-rs/clap/pulls/2859.md) on 2021-10-12 16:17_

---
