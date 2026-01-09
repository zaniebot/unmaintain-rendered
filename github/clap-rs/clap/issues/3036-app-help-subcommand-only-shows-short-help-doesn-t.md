---
number: 3036
title: "`app help subcommand` only shows short help, doesn't include long_about"
type: issue
state: closed
author: gibfahn
labels:
  - C-bug
assignees: []
created_at: 2021-11-18T11:33:24Z
updated_at: 2021-11-18T21:03:24Z
url: https://github.com/clap-rs/clap/issues/3036
synced_at: 2026-01-07T13:12:19-06:00
---

# `app help subcommand` only shows short help, doesn't include long_about

---

_Issue opened by @gibfahn on 2021-11-18 11:33_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.1 (59eed8a2a 2021-11-01)

### Clap Version

3.0.0-beta.5

### Minimal reproducible code

```rust
use clap::Parser;

fn main() {
    Opts::parse();
}

#[derive(Parser)]
#[clap(version)]
pub struct Opts {
    #[clap(subcommand)]
    pub cmd: Subcommand,
}

#[derive(Debug, Parser)]
pub enum Subcommand {
    /**
    First line docs for subcommand foo, always shown.

    Extra docs for subcommand foo, only shown in long help.

    `clap_repro foo --help` -> long help (as expected)
    `clap_repro foo -h` -> short help (as expected)
    `clap_repro help foo` -> short help (unexpected)

    Examples:

    ❯ clap_repro foo # Does nothing.
    */
    Foo,
}
```

(this code is at https://github.com/gibfahn/clap_repro/tree/short_help_only for ease of copying)


### Steps to reproduce the bug with the above code

```shell
cargo run -- help foo
```

### Actual Behaviour

(basing my terminology on https://github.com/clap-rs/clap/issues/2983#issuecomment-957999783)

Clap only shows the `about`, not the `long_about`. Same output as if you run `cargo run -- foo -h`.

### Expected Behaviour

Clap should (I think) also show the `long_about`. This would be the same output as if you were to run `cargo run -- foo --help`.

### Additional Context

I think users who run `app help subcommand` are expecting to get the full help, at least for me `help <subcommand>` seems equivalent to `<subcommand> --help`, it's just easier to write.

### Debug Output

<details><summary>Debug output of <code>cargo run -- help foo</code></summary>

```console
❯ cargo run -- help foo
    Finished dev [unoptimized + debuginfo] target(s) in 0.55s
     Running `target/debug/clap_repro help foo`
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:clap_repro
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Building help subcommand
[            clap::build::app] 	App::_propagate_global_args:clap_repro
[            clap::build::app] 	App::_derive_display_order:clap_repro
[            clap::build::app] 	App::_derive_display_order:foo
[            clap::build::app] 	App::_derive_display_order:help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("help")' ([104, 101, 108, 112])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("help")
[         clap::parse::parser] 	Parser::get_matches_with: sc=Some("help")
[         clap::parse::parser] 	Parser::parse_help_subcommand
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:foo
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:foo
[            clap::build::app] 	App::_derive_display_order:foo
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[         clap::parse::parser] 	Parser::help_err: use_long=false
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage] 	Usage::create_help_usage: usage=clap_repro foo
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_after_help
[           clap::output::fmt] 	is_a_tty: stderr=false
clap_repro-foo 

First line docs for subcommand foo, always shown

USAGE:
    clap_repro foo

OPTIONS:
    -h, --help    Print help information
```

</details>

---

_Label `T: bug` added by @gibfahn on 2021-11-18 11:33_

---

_Comment by @mkayaalp on 2021-11-18 20:38_

https://docs.rs/clap/3.0.0-beta.5/clap/enum.AppSettings.html#variant.UseLongFormatForHelpSubcommand
```diff
- #[clap(version)]
+ #[clap(version, setting(AppSettings::UseLongFormatForHelpSubcommand))]
```

---

_Locked by @clap-rs on 2021-11-18 21:03_

---

_Closed by @pksunkara on 2021-11-18 21:03_

---
