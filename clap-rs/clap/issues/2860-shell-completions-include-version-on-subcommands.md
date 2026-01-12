```yaml
number: 2860
title: "Shell completions include `--version` on subcommands despite no version set"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2021-10-12T17:01:27Z
updated_at: 2021-10-12T22:31:15Z
url: https://github.com/clap-rs/clap/issues/2860
synced_at: 2026-01-12T16:14:13Z
```

# Shell completions include `--version` on subcommands despite no version set

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

This snippet is using the API the generators used, so it can demonstrate the problem

```rust
fn main() {
    use clap::*;
    let mut app = App::new("fig")
        .about("Tests completions")
        .arg(
            Arg::new("file")
                .value_hint(ValueHint::FilePath)
                .about("some input file"),
        )
        .subcommand(
            App::new("test").about("tests things").arg(
                Arg::new("case")
                    .long("case")
                    .takes_value(true)
                    .about("the case to test"),
            ),
        );

    app.get_matches_mut();
    for flag in clap_generate::utils::flags(&app) {
        if flag.get_name() == "version" {
            dbg!(flag);
        }
    }

    for subcommand in app.get_subcommands() {
        for flag in clap_generate::utils::flags(&subcommand) {
            if flag.get_name() == "version" {
                dbg!(subcommand.get_name());
                dbg!(flag);
            }
        }
    }
}

```


### Steps to reproduce the bug with the above code

Run it

### Actual Behaviour

See a `version` flag included for each subcommand

### Expected Behaviour

No output should happen

### Additional Context

This was found as part of #2858.

If I'm reading this correctly, #2831 relied on `fn _build` to remove `--version` in the call to `_check_help_and_version` **but** we don't recursively call `_build` on subcommands, leaving the auto-generated flag behind.

if this is the case, then probably a lot of subcommand behavior is broken because we'd never call `_build` on their args.

### Debug Output

```
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:fig
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_check_help_and_version: Building help subcommand
[            clap::build::app]  App::_propagate_global_args:fig
[            clap::build::app]  App::_derive_display_order:fig
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_derive_display_order:help
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:file
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::remove_overrides
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_env: Checking arg `--help`
[         clap::parse::parser]  Parser::add_env: Checking arg `<file>`
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:file:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:file: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:file: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help]
```

Note the single logged `App::_build`, rather than one-per-subcommand.

---

_Label `T: bug` added by @epage on 2021-10-12 17:01_

---

_Label `C: generator` added by @epage on 2021-10-12 17:01_

---

_Added to milestone `3.0` by @epage on 2021-10-12 17:01_

---

_Comment by @pksunkara on 2021-10-12 17:11_

Oof, I forgot about tracking this issue. The context is:

We do lazy building of the subcommand to improve performance in our parser once we match a subcommand. But generators need all the subcommands to be built. I have a `TODO` marked [here](https://github.com/clap-rs/clap/blob/master/clap_generate/src/lib.rs#L253) about this.

---

_Comment by @epage on 2021-10-12 17:15_

Thanks for the pointer!

---

_Assigned to @epage by @epage on 2021-10-12 17:16_

---

_Referenced in [clap-rs/clap#2863](../../clap-rs/clap/pulls/2863.md) on 2021-10-12 20:19_

---

_Closed by @bors[bot] on 2021-10-12 22:31_

---
