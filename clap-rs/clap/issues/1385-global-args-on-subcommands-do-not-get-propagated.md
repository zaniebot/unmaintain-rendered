---
number: 1385
title: Global args on subcommands do not get propagated to sub-subcommands
type: issue
state: closed
author: ctron
labels:
  - C-bug
assignees: []
created_at: 2018-11-19T12:24:34Z
updated_at: 2020-12-14T02:20:35Z
url: https://github.com/clap-rs/clap/issues/1385
synced_at: 2026-01-10T01:26:51Z
---

# Global args on subcommands do not get propagated to sub-subcommands

---

_Issue opened by @ctron on 2018-11-19 12:24_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

~~~
rustc 1.30.1 (1433507eb 2018-11-07)
~~~

### Affected Version of clap

~~~
2.32.0
~~~

### Bug or Feature Request Summary

Bug

### Expected Behavior Summary

Subcommand matches gets value.

### Actual Behavior Summary

Subcommand matched do not get value.

### Steps to Reproduce the issue

Run sample code.

### Sample Code or Link to Sample Code

~~~rust
extern crate clap;

use clap::{Arg,App,SubCommand};

fn main() {

    let app = App::new("foo")

        .subcommand(SubCommand::with_name("sub1")

            .arg(Arg::with_name("arg1")
                .long("arg1")
                .takes_value(true)
                .global(true)
            )

            .subcommand(SubCommand::with_name("sub1a")
            )
        )
    ;

    let m1 = app.get_matches_from(&["foo", "sub1", "--arg1", "v1", "sub1a"]); // fails

    println!("{:#?}", m1);

    let v1 = m1.subcommand_matches("sub1").unwrap().subcommand_matches("sub1a").unwrap().value_of("arg1").unwrap();
    println!("{}", v1);
}
~~~

On the example application I would expect the subcommand to receive the global value as well. Putting the argument `arg1` on the app global level works. But then the argument is available outside the subcommand `sub1` as well.

### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2", features = ["debug"] }
```

<details>
<summary> Debug Output </summary>
<pre>
<code>

   Compiling clap v2.32.0
   Compiling clap1 v0.1.0 (/home/jreimann/git/clap1)
    Finished dev [unoptimized + debuginfo] target(s) in 8.18s
     Running `target/debug/clap1`
DEBUG:clap:Parser::add_subcommand: term_w=None, name=sub1a
DEBUG:clap:Parser::add_subcommand: term_w=None, name=sub1
DEBUG:clap:Parser::propagate_settings: self=foo, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=sub1, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=sub1, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=sub1a, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=sub1a, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"sub1"' ([115, 117, 98, 49])
DEBUG:clap:Parser::is_new_arg:"sub1":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="sub1"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("sub1")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=sub1
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--arg1"' ([45, 45, 97, 114, 103, 49])
DEBUG:clap:Parser::is_new_arg:"--arg1":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--arg1"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:OptBuilder::fmt:arg1
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--arg1 <arg1>'
DEBUG:clap:Parser::parse_opt; opt=arg1, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(EMPTY_VALS | GLOBAL | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=arg1
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=arg1
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Opt("arg1")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"v1"' ([118, 49])
DEBUG:clap:Parser::is_new_arg:"v1":Opt("arg1")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=arg1, val="v1"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."v1"
DEBUG:clap:Parser::groups_for_arg: name=arg1
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=arg1
DEBUG:clap:Parser::get_matches_with: Begin parsing '"sub1a"' ([115, 117, 98, 49, 97])
DEBUG:clap:Parser::is_new_arg:"sub1a":ValuesDone
DEBUG:clap:Parser::possible_subcommand: arg="sub1a"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("sub1a")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=["arg1"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["arg1"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=sub1a
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:arg1: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:arg1: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=foo sub1 sub1a [OPTIONS]
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("arg1");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:arg1: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:arg1: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:arg1;
DEBUG:clap:Parser::groups_for_arg: name=arg1
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:arg1: vals=[
    "v1"
]
DEBUG:clap:Validator::validate_arg_num_vals:arg1
DEBUG:clap:Validator::validate_arg_values: arg="arg1"
DEBUG:clap:Validator::validate_arg_requires:arg1;
DEBUG:clap:Validator::validate_arg_num_occurs: a=arg1;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=foo sub1 [OPTIONS] [SUBCOMMAND]
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=foo [SUBCOMMAND]
DEBUG:clap:ArgMatcher::get_global_values: global_arg_vec=[]
ArgMatches {
    args: {},
    subcommand: Some(
        SubCommand {
            name: "sub1",
            matches: ArgMatches {
                args: {
                    "arg1": MatchedArg {
                        occurs: 1,
                        indices: [
                            2
                        ],
                        vals: [
                            "v1"
                        ]
                    }
                },
                subcommand: Some(
                    SubCommand {
                        name: "sub1a",
                        matches: ArgMatches {
                            args: {},
                            subcommand: None,
                            usage: Some(
                                "USAGE:\n    foo sub1 sub1a [OPTIONS]"
                            )
                        }
                    }
                ),
                usage: Some(
                    "USAGE:\n    foo sub1 [OPTIONS] [SUBCOMMAND]"
                )
            }
        }
    ),
    usage: Some(
        "USAGE:\n    foo [SUBCOMMAND]"
    )
}
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', libcore/option.rs:345:21
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:211
   3: std::panicking::default_hook
             at libstd/panicking.rs:227
   4: std::panicking::rust_panic_with_hook
             at libstd/panicking.rs:477
   5: std::panicking::continue_panic_fmt
             at libstd/panicking.rs:391
   6: rust_begin_unwind
             at libstd/panicking.rs:326
   7: core::panicking::panic_fmt
             at libcore/panicking.rs:77
   8: core::panicking::panic
             at libcore/panicking.rs:52
   9: <core::option::Option<T>>::unwrap
             at libcore/macros.rs:20
  10: clap1::main
             at src/main.rs:27
  11: std::rt::lang_start::{{closure}}
             at libstd/rt.rs:74
  12: std::panicking::try::do_call
             at libstd/rt.rs:59
             at libstd/panicking.rs:310
  13: __rust_maybe_catch_panic
             at libpanic_unwind/lib.rs:103
  14: std::rt::lang_start_internal
             at libstd/panicking.rs:289
             at libstd/panic.rs:392
             at libstd/rt.rs:58
  15: std::rt::lang_start
             at libstd/rt.rs:74
  16: main
  17: __libc_start_main
  18: <unknown>

Process finished with exit code 101


</code>
</pre>
</details>


---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 15:28_

---

_Renamed from "Global args on subcommands do not get propagated" to "Global args on subcommands do not get propagated to sub-subcommands" by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `C: args` added by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `D: intermediate` added by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-06 09:43_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 09:43_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-06 09:43_

---

_Comment by @pksunkara on 2020-04-09 07:48_

When you say its a global arg, it means you can specify it in any subcommand, not that you actually get that value in the subcommand.

```rust
&["foo", "sub1", "sub1a", "--arg1", "v1"]
```

@CreepySkeleton Do you agree?

---

_Comment by @CreepySkeleton on 2020-04-09 20:24_

@pksunkara Speaking for my personal intuition, I would expect global args to be accessible from everywhere. "Global" is in the tittle, anyway.

Not really sure if it would worth the effort, but I'd like to see it implemented. I'll be looking into it when I get to bugfixing, I'll decide then.

---

_Referenced in [geldata/rfcs#14](../../geldata/rfcs/pulls/14.md) on 2020-07-03 09:15_

---

_Referenced in [clap-rs/clap#2026](../../clap-rs/clap/pulls/2026.md) on 2020-07-26 07:39_

---

_Referenced in [clap-rs/clap#2053](../../clap-rs/clap/issues/2053.md) on 2020-08-06 18:57_

---

_Comment by @pksunkara on 2020-10-25 22:03_

Might be related to #2053 

---

_Comment by @ldm0 on 2020-12-12 05:58_

Well,
This works, 
```rust
    let m1 = App::new("foo")
        .arg(Arg::new("arg1").long("arg1").takes_value(true).global(true))
        .subcommand(
            App::new("sub1")
                .subcommand(App::new("sub1a")),
        )
```
but this doesn't:
```rust
    let m1 = App::new("foo")
        .subcommand(
            App::new("sub1")
                .arg(Arg::new("arg1").long("arg1").takes_value(true).global(true))
                .subcommand(App::new("sub1a")),
        )
```

---

_Referenced in [clap-rs/clap#2253](../../clap-rs/clap/pulls/2253.md) on 2020-12-12 13:42_

---

_Closed by @pksunkara on 2020-12-14 02:20_

---

_Referenced in [hyperledger-archives/grid#997](../../hyperledger-archives/grid/issues/997.md) on 2021-10-27 21:49_

---
