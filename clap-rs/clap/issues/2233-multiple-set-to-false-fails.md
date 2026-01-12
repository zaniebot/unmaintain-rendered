```yaml
number: 2233
title: Multiple set to false fails
type: issue
state: closed
author: crowlKats
labels:
  - C-bug
assignees: []
created_at: 2020-11-29T13:38:52Z
updated_at: 2021-03-09T17:49:06Z
url: https://github.com/clap-rs/clap/issues/2233
synced_at: 2026-01-12T16:14:12Z
```

# Multiple set to false fails

---

_@crowlKats_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
let app = App::new("test").arg(
  Arg::new("foo")
    .long("foo")
    .takes_value(true)
    .multiple(false),
);

println!("{}", app.get_matches().value_of("foo").unwrap());
```

### Steps to reproduce the issue

Run `cargo run -- --foo=bar` or `cargo run -- --foo bar`

### Version

* **Rust**: 1.48.0
* **Clap**: master

### Actual Behavior Summary

using `=` it crashes with
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/main.rs:11:52
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
whereas without it, it fails with
```
error: Found argument 'bar' which wasn't expected, or isn't valid in this context

If you tried to supply `bar` as a PATTERN use `-- bar`

USAGE:
    untitled [FLAGS]

For more information try --help
```

This currently hinders Deno to upgrade clap to 3.0.0-beta.2. https://github.com/denoland/deno/pull/8485

### Expected Behavior Summary

It should behave as it did in v2, so not fail and give the actual value

### Additional context

Add any other context about the problem here.

### Debug output

<details>
<summary> Debug Output without = </summary>
<pre>
<code>
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:test
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:foo
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"--foo"' ([45, 45, 102, 111, 111])
[         clap::parse::parser]  Parser::is_new_arg: "--foo":NotFound
[         clap::parse::parser]  Parser::is_new_arg: want_value=false
[         clap::parse::parser]  Parser::is_new_arg: -- found
[         clap::parse::parser]  Parser::possible_subcommand: arg="--foo"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--foo'
[         clap::parse::parser]  Parser::check_for_help_and_version_str
[         clap::parse::parser]  Parser::check_for_help_and_version_str: Checking if --"foo" is help or version...
[         clap::parse::parser]  Neither
[         clap::parse::parser]  Parser::parse_flag
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=foo
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: first instance
[            clap::build::app]  App::groups_for_arg: id=foo
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Flag(foo)
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: arg = foo
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: is it positional?
[         clap::parse::parser]  No
[            clap::build::app]  App::groups_for_arg: id=foo
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"bar"' ([98, 97, 114])
[         clap::parse::parser]  Parser::is_new_arg: "bar":Flag(foo)
[         clap::parse::parser]  Parser::is_new_arg: want_value=false
[         clap::parse::parser]  Parser::is_new_arg: value
[         clap::parse::parser]  Parser::possible_subcommand: arg="bar"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage]  Usage::needs_flags_tag
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=foo
[            clap::build::app]  App::groups_for_arg: id=foo
[         clap::output::usage]  Usage::needs_flags_tag:iter: [FLAGS] required
[         clap::output::usage]  Usage::create_help_usage: usage=untitled [FLAGS]
[            clap::build::app]  App::color: Color setting...
[            clap::build::app]  Auto
[           clap::output::fmt]  is_a_tty: stderr=true
</code>
</pre>
</details>

<details>
<summary> Debug Output with = </summary>
<pre>
<code>
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:test
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:foo
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"--foo=bar"' ([45, 45, 102, 111, 111, 61, 98, 97, 114])
[         clap::parse::parser]  Parser::is_new_arg: "--foo=bar":NotFound
[         clap::parse::parser]  Parser::is_new_arg: want_value=false
[         clap::parse::parser]  Parser::is_new_arg: -- found
[         clap::parse::parser]  Parser::possible_subcommand: arg="--foo=bar"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  Yes '"=bar"'
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--foo'
[         clap::parse::parser]  Parser::check_for_help_and_version_str
[         clap::parse::parser]  Parser::check_for_help_and_version_str: Checking if --"foo" is help or version...
[         clap::parse::parser]  Neither
[         clap::parse::parser]  Parser::parse_flag
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=foo
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: first instance
[            clap::build::app]  App::groups_for_arg: id=foo
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Flag(foo)
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: arg = foo
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: is it positional?
[         clap::parse::parser]  No
[            clap::build::app]  App::groups_for_arg: id=foo
[         clap::parse::parser]  Parser::remove_overrides
[         clap::parse::parser]  Parser::remove_overrides:iter:foo
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:foo
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::gather_conflicts:iter: id=foo
[            clap::build::app]  App::groups_for_arg: id=foo
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:foo
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:foo: vals=[]
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="foo"
[      clap::parse::validator]  Validator::validate_arg_requires:"foo"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "foo"=1
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
</code>
</pre>
</details>

---

_Label `T: bug` added by @crowlKats on 2020-11-29 13:38_

---

_Referenced in [denoland/deno#8485](../../denoland/deno/pulls/8485.md) on 2020-11-29 13:39_

---

_Added to milestone `3.0` by @pksunkara on 2020-11-29 13:39_

---

_Comment by @ldm0 on 2020-12-09 19:26_

Well, this sounds strange but currently the order of clap's settings does matter. This works:
```rust
let app = App::new("test").arg(
  Arg::new("foo")
    .long("foo")
    .multiple(false)
    .takes_value(true)
);
```
That's becase disable `multiple` actually disables `takes_value` and `multiple`, which shares the same logic that enable `multiple` actually enables `takes_value` and `multiple`. What do you think? @pksunkara 

---

_Comment by @pksunkara on 2020-12-09 20:00_

We need to fix all these inter linking changes.

---

_Label `C: args` added by @pksunkara on 2020-12-09 20:01_

---

_Comment by @ldm0 on 2020-12-10 05:38_

@pksunkara  I don't think currently removing all interlinking changes is a good idea, it may introduces quite a bit resistance on upgrading v2 to v3.

What's about changing the behaviour of setting false. I mean enabling `multiple` enables `takes_value` and `multiple`, while disabling `multiple` only disables `multiple`. It's less surprising for users.

This changes maintains backward compatibility, since nobody(I guess...) depends on the behaviour that disabling `multiple` disables `takes_value`.



---

_Comment by @pksunkara on 2020-12-14 02:30_

We will have https://github.com/pksunkara/cargo-up ready. We shouldn't concern ourselves with either backward compatibility or user churn.

But we can definitely improve the user experience by asserting that `multiple = true, takes_value = false` doesn't happen.

---

_Referenced in [clap-rs/clap#2360](../../clap-rs/clap/pulls/2360.md) on 2021-02-23 16:38_

---

_Comment by @ldm0 on 2021-02-23 16:43_

> We will have https://github.com/pksunkara/cargo-up ready. We shouldn't concern ourselves with either backward compatibility or user churn.
> 
> But we can definitely improve the user experience by asserting that `multiple = true, takes_value = false` doesn't happen.

Opinion syncing: so removing all interlinking flags, and check them in `assert_app`?

---

_Comment by @pksunkara on 2021-02-23 16:45_

Yup.

---

_Referenced in [clap-rs/clap#2362](../../clap-rs/clap/pulls/2362.md) on 2021-02-24 15:45_

---

_Closed by @pksunkara on 2021-03-09 17:49_

---

_Referenced in [SeaQL/sea-orm#664](../../SeaQL/sea-orm/issues/664.md) on 2022-04-13 11:23_

---

_Referenced in [solana-labs/solana-program-library#4511](../../solana-labs/solana-program-library/issues/4511.md) on 2023-06-08 23:26_

---
