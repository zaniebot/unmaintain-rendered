```yaml
number: 1644
title: "Fatal internal error when arg requirement doesn't exist"
type: issue
state: closed
author: sharnoff
labels:
  - E-help-wanted
assignees: []
created_at: 2020-01-26T19:39:15Z
updated_at: 2020-04-10T02:00:15Z
url: https://github.com/clap-rs/clap/issues/1644
synced_at: 2026-01-12T16:14:11Z
```

# Fatal internal error when arg requirement doesn't exist

---

_@sharnoff_

### Rust Version
`rustc 1.40.0 (73528e339 2019-12-16)`

### Affected Version of clap

Both `2.33.0` and `3.0.0-beta.1` (from git)

### Expected Behavior Summary

Clap should complain (to the writer of the app - not the end user of the interface) that one of the arguments requires an `Arg` that does not exist - i.e. has not been added to the `App`.

### Actual Behavior Summary

Fatal internal error.

### Steps to Reproduce the issue

See sample code below.

### Sample Code or Link to Sample Code

```rust
fn main() {
    // The issue also persists when loading from yaml (where I originally found it)
    // the only difference is that if we don't give a name for the App, it doesn't
    // panic.
    let app = clap::App::new("clap-issue-1644")
        .arg(clap::Arg::with_name("foo")
                // It doesn't matter whether we use a short or long value here;
                // clap still crashes
                .long("foo")
                .requires("missing-arg"));

    app.get_matches();
}
```

### Debug output

<details>
<summary> Debug Output (v2.33.0) </summary>
<pre>
<code>
DEBUG:clap:Parser::propagate_settings: self=clap-issue-1644, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--foo"' ([45, 45, 102, 111, 111])
DEBUG:clap:Parser::is_new_arg:"--foo":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--foo"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--foo'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --foo is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=foo
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Flag
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("foo");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:foo;
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:foo: vals=[]
DEBUG:clap:Validator::validate_arg_requires:foo;
DEBUG:clap:Validator::missing_required_error: extra=Some("mising-arg")
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "mising-arg",
]
DEBUG:clap:usage::get_required_usage_from: reqs=["mising-arg"], extra=Some("mising-arg")
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=["mising-arg"]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["mising-arg"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:mising-arg:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', src/libcore/option.rs:1185:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
</code>
</pre>
</details>

<details>
<summary> Debug Output (v3.0.0-beta.1) </summary>
<pre>
<code>
DEBUG:clap:App::_do_parse;
DEBUG:clap:App::_build;
DEBUG:clap:App::_derive_display_order:clap-issue-1644
DEBUG:clap:App::_create_help_and_version;
DEBUG:clap:App::_create_help_and_version: Building --help
DEBUG:clap:App::_create_help_and_version: Building --version
DEBUG:clap:App::_arg_debug_asserts:foo
DEBUG:clap:App::_arg_debug_asserts:help
DEBUG:clap:App::_arg_debug_asserts:version
DEBUG:clap:App::_app_debug_asserts;
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::_build;
DEBUG:clap:Parser::_verify_positionals;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--foo"' ([45, 45, 102, 111, 111])
DEBUG:clap:Parser::is_new_arg:"--foo":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--foo"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid opt or flag '--foo'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --foo is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=4741034841092048056
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=4741034841092048056
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Flag
DEBUG:clap:Parser::remove_overrides;
DEBUG:clap:Parser::remove_overrides:iter:4741034841092048056;
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_conflicts;
DEBUG:clap:Validator::validate_conflicts_with_everything;
DEBUG:clap:Validator::validate_conflicts_with_everything:iter:4741034841092048056;
DEBUG:clap:Validator::gather_conflicts;
DEBUG:clap:Validator::gather_conflicts:iter:4741034841092048056;
DEBUG:clap:Parser::groups_for_arg: name=4741034841092048056
DEBUG:clap:Validator::validate_required: required=ChildGraph([]);
DEBUG:clap:Validator::gather_requirements;
DEBUG:clap:Validator::gather_requirements:iter:4741034841092048056;
DEBUG:clap:Validator::validate_required:iter:aog=18152889946298061510;
DEBUG:clap:Validator::validate_required_unless;
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:4741034841092048056: vals=[]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_arg_values: arg="foo"
DEBUG:clap:Validator::validate_arg_requires:foo;
DEBUG:clap:Validator::missing_required_error; incl=Some(18152889946298061510)
DEBUG:clap:App::color;
DEBUG:clap:App::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=ChildGraph([Child { id: 18152889946298061510, children: None }])
DEBUG:clap:Usage::get_required_usage_from: incls=[18152889946298061510], matcher=true, incl_last=true
DEBUG:clap:Usage::get_required_usage_from:iter:18152889946298061510:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', src/libcore/option.rs:1185:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
</code>
</pre>
</details>

---

_Comment by @CreepySkeleton on 2020-02-01 10:16_

I think that panicking is exactly what we need to do here, this *is* the way to inform developer of problems like this one.  

But we do need to rework panic messages in debug builds.

---

_Added to milestone `v3.0.0-alpha.3` by @CreepySkeleton on 2020-02-01 10:46_

---

_Label `C: errors` added by @CreepySkeleton on 2020-02-01 10:47_

---

_Label `D: easy` added by @CreepySkeleton on 2020-02-01 10:47_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 10:47_

---

_Referenced in [clap-rs/clap#1479](../../clap-rs/clap/issues/1479.md) on 2020-02-01 13:37_

---

_Comment by @pksunkara on 2020-02-01 22:36_

Looks similar to #990 

---

_Comment by @CreepySkeleton on 2020-02-02 02:21_

First, the ` Please consider filing a bug report` part should be omitted in debug builds, this message is aimed at users but not developers.

Second, the backtrace is good, but the error should be more descriptive in my opinion.

---

_Comment by @Dylan-DPC-zz on 2020-02-02 14:59_

> First, the Please consider filing a bug report part should be omitted in debug builds, this message is aimed at users but not developers.

For debug build, our users are our developers 😄

---

_Comment by @CreepySkeleton on 2020-02-09 10:47_

Here's one another example why we need more descriptive messages https://github.com/TeXitoi/structopt/issues/348

---

_Comment by @sharnoff on 2020-02-09 12:02_

Is there a rough idea of what should be done here? If so, I'd be happy write up a pull request

---

_Comment by @CreepySkeleton on 2020-02-09 14:35_

This is complicated.

People who encounter an error originating from `clap` can be divided into two groups:
* Developers who use clap to build their own programs, e.g people who use clap as library. Let's call them `developers`.
* End users who use programs written by `developers`, let's call them `users`.

There are three types of errors:
* Bugs in `clap` itself, `clap-bug`
* Bugs in programs written by `developers`, `prog-bug`.
* Malformed input from `users`, `input`.

`developers` are interested in seeing good descriptive panics (and not just "BOOM, program panicked with clap internal error") when it comes to `prog-bug` errors; besides, it would be good to direct them to clap bugtracker in case they encounter `clap-bug`. 

`users` don't want to think about it, they only need a link to bugtracker. I think we should give `developers` a way to specify their own bugtracker so `users` would go there.

Something like this:

|                   | `clap-bug`                   | `prog-bug` | `input`
| -- | -- | -- | -- |
| `developers` | `panic` with  backtrace + link to `clap` bugtracker | `panic` with backtrace and detailed error description | N/A
| `users` | `panic` with backtrace + link to `clap` bugtracker | `panic` with backtrace and link to `developer`-specified bugtracker |  `exit(2)` + print error  |

cc @clap-rs/admins @clap-rs/cli-wg  thoughts?

---

_Comment by @pksunkara on 2020-03-13 11:56_

So, I think `clap-bug` is completely covered as of now in our codebase. We can cover `prog-bug` of `developers` by doing `compile_error` when building the app using `_build`. Finding out `prog-bug` of `users` is a difficult thing here. We can't differentiate. I don't think we should be using `debug` for providing any errors. It should be more about how the program is working.

For example, if the developer writes an argument that is conflicting with itself, we should emit a compilation error, that way they would realize it is not correct code.

---

_Comment by @CreepySkeleton on 2020-03-13 13:44_

> We can cover prog-bug of developers by doing compile_error when building the app using _build

You're apparently misunderstanding how it works. `compile_error!` aborts the compilation whenever it ends up present in the code after `cfg`s has been expanded. [Example](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=ce3774f3c074e7b02fb6857b815e5a74). You can't do "if some condition is not met - emit compilation error".

It has to be panic because nothing else is available.

>  Finding out prog-bug of users is a difficult thing here. We can't differentiate.

In general case - no, we cant. In *most* cases, like [that one](https://github.com/clap-rs/clap/issues/1743) - yes, we very well can.

>  I don't think we should be using debug for providing any errors. It should be more about how the program is working.

You mean, get rid of the "debug" feature?



---

_Comment by @pksunkara on 2020-03-13 13:51_

> It has to be panic because nothing else is available.

My bad then. Yeah, we should panic.

> In general case - no, we cant. In most cases, like that one - yes, we very well can.

That's actually a prog-bug developer. Not prog-bug user.

> You mean, get rid of the "debug" feature?

No. I am saying the above mentioned panics should always happen instead of only happening when debug is present. Basically, prog-bug developer and debug are separate things.

---

_Comment by @CreepySkeleton on 2020-03-13 14:05_

> That's actually a prog-bug developer. Not prog-bug user.

There's no such distinction. It's always one of the two:
* We made an error in clap itself (`clap-bug`)
* A developer made an error in they're own program (`prog-bug`)



---

_Comment by @pksunkara on 2020-03-13 14:09_

I agree. I was just trying to distinguish things a bit more based on [this](https://github.com/clap-rs/clap/issues/1644#issuecomment-583852176) comment and we don't really need that.

We agree on panicking during app building, right?

---

_Comment by @CreepySkeleton on 2020-03-13 14:11_

Right

---

_Removed from milestone `3.0.0-beta.2` by @pksunkara on 2020-04-07 13:59_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-07 13:59_

---

_Label `C: asserts` added by @pksunkara on 2020-04-09 08:14_

---

_Closed by @bors[bot] on 2020-04-10 02:00_

---
