```yaml
number: 2714
title: "`requires` and `default_value` override the `required` field on the target of `requires`"
type: issue
state: closed
author: rectangular-zone
labels:
  - C-bug
assignees: []
created_at: 2021-08-18T03:32:08Z
updated_at: 2021-10-27T21:31:07Z
url: https://github.com/clap-rs/clap/issues/2714
synced_at: 2026-01-12T16:14:13Z
```

# `requires` and `default_value` override the `required` field on the target of `requires`

---

_@rectangular-zone_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.54

### Clap Version

3.0.0-beta.4

### Minimal reproducible code

```rust
use clap::{App, Arg};

mod flags {
    pub const OPTIONAL: &str = "optional";
    pub const REQUIRES_OPTIONAL: &str = "requires-optional";
}

fn main() {
    let _ = App::new("mwe")
        .args(&[
            Arg::new(flags::OPTIONAL)
                .long("optional")
                .required(false)
                .about("This flag should be optional unless the other flag is provided"),
            Arg::new(flags::REQUIRES_OPTIONAL)
                .takes_value(true)
                .required(false)
                .requires(flags::OPTIONAL)
                .default_value("some default")
                .about("This flag forces the optional field to be required if the default value is set"),
        ])
        .get_matches();

    println!("Demo");
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

If `default_value` is set on the arg that specifies `requires`, then it forces the target of `requires` to be required, even if `required(false)` is specified, or `required` is omitted entirely.
```
$ cargo run
   Compiling mwe v0.1.0 (/tmp/mwe)
    Finished dev [unoptimized + debuginfo] target(s) in 0.34s
     Running `target/debug/mwe`
error: The following required arguments were not provided:
    --optional

USAGE:
    mwe [FLAGS] --optional [requires-optional]

For more information try --help
```


### Expected Behaviour

So, personally: I don't think specifying a default value should have this effect. It feels surprising, and a little un-intuitive. That said, it also makes a kind of sense. If the response here is, "no, that works correctly," that's cool! Maybe y'all could point me at a place I could PR a docs update then, make sure this is written down somewhere? (Or! Maybe I'm going to be incredibly sheepish when y'all point out where it's already documented.)

What I'm going for is: if you specify `b`, then `a` must also be specified, while if `a` is specified, `b` gets a sensible default. 

### Additional Context

Nope! Clap is an amazing project, I'm thrilled to be able to use it. 

### Debug Output

[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:mwe
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_propagate_global_args:mwe
[            clap::build::app] 	App::_derive_display_order:mwe
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:version
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:optional
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:requires-optional
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_env: Checking arg `--help`
[         clap::parse::parser] 	Parser::add_env: Checking arg `--version`
[         clap::parse::parser] 	Parser::add_env: Checking arg `--optional`
[         clap::parse::parser] 	Parser::add_env: Checking arg `<requires-optional>`
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:requires-optional:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:requires-optional: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:requires-optional: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=requires-optional
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."some default"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[            clap::build::app] 	App::groups_for_arg: id=requires-optional
[         clap::parse::parser] 	Parser::add_value:iter:requires-optional: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:requires-optional
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=requires-optional
[      clap::parse::validator] 	Validator::gather_conflicts:iter: This is default value, skipping.
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:requires-optional
[      clap::parse::validator] 	Validator::validate_required:iter:aog=optional
[      clap::parse::validator] 	Validator::validate_required:iter: This is an arg
[      clap::parse::validator] 	Validator::is_missing_required_ok: optional
[      clap::parse::validator] 	Validator::validate_arg_conflicts: a="optional"
[      clap::parse::validator] 	Validator::missing_required_error; incl=[]
[      clap::parse::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: optional, children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[optional]
[         clap::output::usage] 	Usage::get_required_usage_from:iter:optional
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["--optional"]
[      clap::parse::validator] 	Validator::missing_required_error: req_args=[
    "--optional",
]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[optional]
[         clap::output::usage] 	Usage::get_required_usage_from:iter:optional
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["--optional"]
[         clap::output::usage] 	Usage::needs_flags_tag
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=version
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=optional
[            clap::build::app] 	App::groups_for_arg: id=optional
[         clap::output::usage] 	Usage::needs_flags_tag:iter: [FLAGS] required
[         clap::output::usage] 	Usage::get_args_tag; incl_reqs = true
[         clap::output::usage] 	Usage::get_args_tag:iter:requires-optional
[            clap::build::app] 	App::groups_for_arg: id=requires-optional
[         clap::output::usage] 	Usage::get_args_tag:iter: 1 Args not required or hidden
[            clap::build::app] 	App::groups_for_arg: id=requires-optional
[         clap::output::usage] 	Usage::get_args_tag:iter: Exactly one, returning 'requires-optional'
[            clap::build::arg] 	Arg::name_no_brackets:requires-optional
[            clap::build::arg] 	Arg::name_no_brackets: just name
[         clap::output::usage] 	Usage::create_help_usage: usage=mwe [FLAGS] --optional [requires-optional]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
[           clap::output::fmt] 	is_a_tty: stderr=true


---

_Label `T: bug` added by @rectangular-zone on 2021-08-18 03:32_

---

_Comment by @epage on 2021-08-18 14:40_

Related symptom with presumably the same root cause:  https://github.com/clap-rs/clap/issues/1586

---

_Added to milestone `3.0` by @pksunkara on 2021-08-18 14:55_

---

_Label `C: arg groups` added by @pksunkara on 2021-08-18 14:55_

---

_Comment by @kbknapp on 2021-10-05 01:23_

While this is working as intended, I also can understand that it's surprising and perhaps not the best approach. Perhaps what needs to happen is we need to re-work [`Arg::requires_if`](https://docs.rs/clap/3.0.0-beta.4/clap/struct.Arg.html#method.requires_if) to take a closure that determines if the other argument is required or not instead of just a `val == some_val` constraint.

I'm not sure that I can get on board with going all the way that this behavior should be changed *by default* though. If the default worked as expected here then implementing a closure style `requires_if` would be much more difficult.

I would however be very on board with calling this edge case out in the docs though for the the `Arg:;requires` (and current `Arg::requires_if`) saying that when used with a default value, the default value counts as the argument being used.

A (temporary) workaround would be to *not* have a default value, and instead do the default value manually:

```rust
use clap::{App, Arg};

mod flags {
    pub const OPTIONAL: &str = "optional";
    pub const REQUIRES_OPTIONAL: &str = "requires-optional";
}

fn main() {
    let m = App::new("mwe")
        .args(&[
            Arg::new(flags::OPTIONAL)
                .long("optional")
                .required(false)
                .about("This flag should be optional unless the other flag is provided"),
            Arg::new(flags::REQUIRES_OPTIONAL)
                .takes_value(true)
                .required(false)
                .requires(flags::OPTIONAL)
                .about("This flag forces the optional field to be required if the default value is set"),
        ])
        .get_matches();

    println!("Default: {}", m.value_of(flags::REQUIRES_OPTIONAL).unwrap_or("some_default"));
}
```


---

_Referenced in [clap-rs/clap#2896](../../clap-rs/clap/pulls/2896.md) on 2021-10-16 18:47_

---

_Comment by @epage on 2021-10-16 18:54_

@kbknapp I just wanted to double check, do you feel the documentation update (#2896) resolves this or just lowers the urgency so it doesn't need to be in 3.0?

---

_Comment by @kbknapp on 2021-10-16 19:01_

@epage yes I think the documentation note is good enough to resolve this issue being that it's currently working as intended (even if there are *potentially* better ways to go about this). If we want to go for a more complete or closure based approach I suggest we open a new issue for that, and it will happen post-v3.

---

_Closed by @kbknapp on 2021-10-16 19:02_

---

_Comment by @pksunkara on 2021-10-16 19:07_

As mentioned in the PR, can we keep this open to look at the design once again?

---

_Comment by @pksunkara on 2021-10-16 19:08_

> closure based approach

What's that?

---

_Comment by @epage on 2021-10-16 19:27_

Personally, I'm ok with either closing this or leaving it open.  I do feel the documentation resolution is good enough for now though.

---

_Comment by @pksunkara on 2021-10-16 19:30_

Okay, I am reopening this then but removing 3.0 milestone.

---

_Reopened by @pksunkara on 2021-10-16 19:30_

---

_Removed from milestone `3.0` by @pksunkara on 2021-10-16 19:30_

---

_Referenced in [clap-rs/clap#1586](../../clap-rs/clap/issues/1586.md) on 2021-10-16 19:38_

---

_Referenced in [clap-rs/clap#2897](../../clap-rs/clap/issues/2897.md) on 2021-10-16 19:48_

---

_Comment by @rectangular-zone on 2021-10-18 15:37_

Thanks y'all -- appreciate the time and help! FWIW, the documentation resolution works great for me :+1: 

---

_Closed by @bors[bot] on 2021-10-27 21:31_

---

_Referenced in [epage/clapng#10](../../epage/clapng/issues/10.md) on 2021-11-17 21:58_

---

_Referenced in [clap-rs/clap#3076](../../clap-rs/clap/issues/3076.md) on 2021-12-07 21:56_

---
