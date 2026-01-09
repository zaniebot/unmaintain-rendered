---
number: 1010
title: PropagateGlobalValuesDown not working when arg has default value
type: issue
state: closed
author: AlexanderThaller
labels:
  - C-bug
assignees: []
created_at: 2017-07-22T22:44:23Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/1010
synced_at: 2026-01-07T13:12:19-06:00
---

# PropagateGlobalValuesDown not working when arg has default value

---

_Issue opened by @AlexanderThaller on 2017-07-22 22:44_

Please use the following template to assist with creating an issue and getting a speedy resolution. If an area is not applicable, feel free to delete the area, or mark with `N/A`

### Rust Version
rustc 1.19.0 (0ade33941 2017-07-17)

### Affected Version of clap
2.25.1

### Expected Behavior Summary
The subcommand arg should inherit the set value instead of using the default value.

### Actual Behavior Summary
The subcommand arg uses the default value even if the parent has a different value.

### Sample Code or Link to Sample Code

```rust
#[test]
fn propagate_vals_down_with_default_vals() {
    let m = App::new("myprog")
        .setting(AppSettings::PropagateGlobalValuesDown)
        .arg(
            Arg::with_name("loglevel")
                .help("loglevel")
                .takes_value(true)
                .short("l")
                .long("loglevel")
                .global(true)
                .default_value("info"),
        )
        .subcommand(SubCommand::with_name("foo"))
        .get_matches_from_safe(vec!["myprog", "-l", "trace", "foo"]);
    assert!(m.is_ok(), "{:?}", m.unwrap_err().kind);
    let m = m.unwrap();
    assert_eq!(m.value_of("loglevel"), Some("trace"));
    let sub_m = m.subcommand_matches("foo").unwrap();
    assert_eq!(sub_m.value_of("loglevel"), Some("trace"));
}
```

Fails with
```
---- propagate_vals_down_with_default_vals stdout ----
        thread 'propagate_vals_down_with_default_vals' panicked at 'assertion failed: `(left == right)` (left: `Some("info")`, right: `Some("trace")`)', tests/app_settings.rs:648

---

_Comment by @AlexanderThaller on 2017-07-22 23:04_

I also wrote a small test programm with the debug feature enabled:

```rust
extern crate clap;

use clap::{App, Arg, SubCommand, AppSettings};

fn main() {
    let m = App::new("myprog")
        .setting(AppSettings::PropagateGlobalValuesDown)
        .arg(
            Arg::with_name("loglevel")
                .help("loglevel")
                .takes_value(true)
                .short("l")
                .long("loglevel")
                .global(true)
                .default_value("info"),
        )
        .subcommand(SubCommand::with_name("foo"))
        .get_matches_from_safe(vec!["myprog", "-l", "trace", "foo"]);

    let m = m.unwrap();
    let sub_m = m.subcommand_matches("foo").unwrap();

    println!("m: {:#?}", m);
    println!("sub_m: {:#?}", sub_m);
}
```

```
DEBUG:clap:Parser::add_subcommand: term_w=None, name=foo
DEBUG:clap:Parser::propogate_settings: self=myprog, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::propogate_settings: sc=foo, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::propogate_settings: self=foo, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-l"' ([45, 108])
DEBUG:clap:Parser::is_new_arg: arg="-l", Needs Val of=NotFound
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-l"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-l"
DEBUG:clap:Parser::parse_short_arg:iter:l
DEBUG:clap:Parser::parse_short_arg:iter:l: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:l: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=loglevel, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(EMPTY_VALS | GLOBAL | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=loglevel
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=loglevel
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Is '--loglevel <loglevel>' in overrides...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have overrides...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have conflicts...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Opt("loglevel")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"trace"' ([116, 114, 97, 99, 101])
DEBUG:clap:Parser::is_new_arg: arg="trace", Needs Val of=Opt("loglevel")
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="trace"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::add_val_to_arg; arg=loglevel, val="trace"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."trace"
DEBUG:clap:Parser::groups_for_arg: name=loglevel
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=loglevel
DEBUG:clap:Parser::get_matches_with: Begin parsing '"foo"' ([102, 111, 111])
DEBUG:clap:Parser::is_new_arg: arg="foo", Needs Val of=ValuesDone
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="foo"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("foo")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=["loglevel"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["loglevel"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=foo
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_val_to_arg; arg=loglevel, val="info"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."info"
DEBUG:clap:Parser::groups_for_arg: name=loglevel
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=loglevel
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Is '--loglevel <loglevel>' in overrides...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have overrides...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have conflicts...No
DEBUG:clap:OptBuilder::fmt:loglevel
DEBUG:clap:arg_post_processing!: Does '--loglevel <loglevel>' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Validator::validate_blacklist: blacklist=[]
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:loglevel: vals=[
    "info"
]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_values: arg="loglevel"
DEBUG:clap:Validator::validate_arg_requires;
DEBUG:clap:Validator::validate_arg_num_occurs: a=loglevel;
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
DEBUG:clap:usage::create_help_usage: usage=myprog foo [OPTIONS]
DEBUG:clap:Validator::validate;
DEBUG:clap:Validator::validate_blacklist: blacklist=[]
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:loglevel: vals=[
    "trace"
]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_values: arg="loglevel"
DEBUG:clap:Validator::validate_arg_requires;
DEBUG:clap:Validator::validate_arg_num_occurs: a=loglevel;
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
DEBUG:clap:usage::create_help_usage: usage=myprog [OPTIONS] [SUBCOMMAND]
DEBUG:clap:ArgMatcher::propagate: arg=loglevel
DEBUG:clap:ArgMatcher::propagate: arg=loglevel
DEBUG:clap:ArgMatcher::propagate: Subcommand wasn't used
m: ArgMatches {
    args: {
        "loglevel": MatchedArg {
            occurs: 1,
            vals: [
                "trace"
            ]
        }
    },
    subcommand: Some(
        SubCommand {
            name: "foo",
            matches: ArgMatches {
                args: {
                    "loglevel": MatchedArg {
                        occurs: 0,
                        vals: [
                            "info"
                        ]
                    }
                },
                subcommand: None,
                usage: Some(
                    "USAGE:\n    myprog foo [OPTIONS]"
                )
            }
        }
    ),
    usage: Some(
        "USAGE:\n    myprog [OPTIONS] [SUBCOMMAND]"
    )
}
sub_m: ArgMatches {
    args: {
        "loglevel": MatchedArg {
            occurs: 0,
            vals: [
                "info"
            ]
        }
    },
    subcommand: None,
    usage: Some(
        "USAGE:\n    myprog foo [OPTIONS]"
    )
}
```

---

_Comment by @kbknapp on 2017-10-04 02:45_

Sorry for the long wait, I've been swamped with work traveling for the past month and a half. This may also be related to #978

Once it's fixed I'll double check that this is still the case and close/fix appropriately. 

---

_Label `C: subcommands` added by @kbknapp on 2017-10-04 02:45_

---

_Label `D: easy` added by @kbknapp on 2017-10-04 02:45_

---

_Label `T: bug` added by @kbknapp on 2017-10-04 02:45_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 02:45_

---

_Comment by @AlexanderThaller on 2017-10-15 10:14_

No worries. I saw the other issue that's where I got option from but was unsure if it was related. Thx for your amazing work. 

---

_Comment by @kbknapp on 2017-10-18 14:44_

@AlexanderThaller could you test this with the current master branch and see if it's fixed? #1070 just merged which may have helped. If not I can look into it.

---

_Referenced in [clap-rs/clap#1072](../../clap-rs/clap/pulls/1072.md) on 2017-10-20 14:20_

---

_Closed by @kbknapp on 2017-10-24 12:01_

---

_Comment by @AlexanderThaller on 2017-11-01 16:42_

Works fine thank you for your work.

---
