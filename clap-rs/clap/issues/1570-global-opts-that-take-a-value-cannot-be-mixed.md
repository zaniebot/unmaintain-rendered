```yaml
number: 1570
title: global opts that take a value cannot be mixed with subcommand
type: issue
state: open
author: ehuss
labels:
  - C-bug
  - E-hard
  - A-parsing
  - S-waiting-on-mentor
assignees: []
created_at: 2019-10-08T18:28:33Z
updated_at: 2023-06-30T12:18:37Z
url: https://github.com/clap-rs/clap/issues/1570
synced_at: 2026-01-12T16:14:11Z
```

# global opts that take a value cannot be mixed with subcommand

---

_@ehuss_

### Rust Version

`rustc 1.38.0 (625451e37 2019-09-23)`

### Affected Version of clap

2.33.0 and 3.0.0-beta.1 (b8819130de876c4abbd451c34df89161a98d23ef)

### Expected Behavior Summary

When a global option that takes a value is used both before and after a subcommand, I would expect that `values_of` would return all the values.  

### Actual Behavior Summary

It only returns the values from the subcommand.  Flags before the subcommand are ignored.

Note: For global flags that do not take a value, `occurrences_of` correctly returns the count of all occurrences both before and after.

### Sample Code or Link to Sample Code

```rust
use clap::{App, Arg};

fn main() {
    let matches = App::new("myapp")
        .arg(
            Arg::with_name("global-opt")
                .short('A')
                .multiple(true)
                .number_of_values(1)
                .global(true),
        )
        .subcommand(App::new("test"))
        .get_matches_from(vec!["myapp", "-Aone", "test", "-Atwo"]);

    let gs = matches.values_of("global-opt").unwrap().collect::<Vec<_>>();
    // I would expect this to be ["one", "two"]
    assert_eq!(gs, vec!["two"]);
}
```

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>
DEBUG:clap:App::_do_parse;
DEBUG:clap:App::_build;
DEBUG:clap:App::_derive_display_order:myapp
DEBUG:clap:App::_derive_display_order:test
DEBUG:clap:App::_create_help_and_version;
DEBUG:clap:App::_create_help_and_version: Building --help
DEBUG:clap:App::_create_help_and_version: Building --version
DEBUG:clap:App::_create_help_and_version: Building help
DEBUG:clap:App::_arg_debug_asserts:global-opt
DEBUG:clap:App::_arg_debug_asserts:help
DEBUG:clap:App::_arg_debug_asserts:version
DEBUG:clap:App::_app_debug_asserts;
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::_build;
DEBUG:clap:Parser::_verify_positionals;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-Aone"' ([45, 65, 111, 110, 101])
DEBUG:clap:Parser::is_new_arg:"-Aone":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-Aone"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-Aone"
DEBUG:clap:Parser::parse_short_arg:iter:A
DEBUG:clap:Parser::parse_short_arg:iter:A: Found valid opt or flag
DEBUG:clap:Parser::parse_short_arg:iter:A: p[0]=[], p[1]=[111, 110, 101]
DEBUG:clap:Parser::parse_short_arg:iter:A: val=[111, 110, 101] (bytes), val="one" (ascii)
DEBUG:clap:Parser::parse_opt; opt=global-opt, val=Some("one")
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL | DELIM_NOT_SET | MULTIPLE_VALS)
DEBUG:clap:Parser::parse_opt; Checking for val...Found - "one", len: 3
DEBUG:clap:Parser::parse_opt: "one" contains '='...false
DEBUG:clap:Parser::add_val_to_arg; arg=global-opt, val="one"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."one"
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:ArgMatcher::needs_more_vals: o=global-opt
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=4551335407501243259
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:ArgMatcher::needs_more_vals: o=global-opt
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:Parser::parse_opt: More arg vals not required...
DEBUG:clap:Parser:get_matches_with: After parse_short_arg ValuesDone
DEBUG:clap:Parser::get_matches_with: Begin parsing '"test"' ([116, 101, 115, 116])
DEBUG:clap:Parser::is_new_arg:"test":ValuesDone
DEBUG:clap:Parser::possible_subcommand: arg="test"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("test")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
DEBUG:clap:Usage::get_required_usage_from: ret_val=[]
DEBUG:clap:App::_propagate:myapp
DEBUG:clap:App::_build;
DEBUG:clap:App::_derive_display_order:test
DEBUG:clap:App::_create_help_and_version;
DEBUG:clap:App::_create_help_and_version: Building --help
DEBUG:clap:App::_create_help_and_version: Building --version
DEBUG:clap:App::_arg_debug_asserts:global-opt
DEBUG:clap:App::_arg_debug_asserts:help
DEBUG:clap:App::_arg_debug_asserts:version
DEBUG:clap:App::_app_debug_asserts;
DEBUG:clap:Parser::parse_subcommand: About to parse sc=test
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::_build;
DEBUG:clap:Parser::_verify_positionals;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-Atwo"' ([45, 65, 116, 119, 111])
DEBUG:clap:Parser::is_new_arg:"-Atwo":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-Atwo"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-Atwo"
DEBUG:clap:Parser::parse_short_arg:iter:A
DEBUG:clap:Parser::parse_short_arg:iter:A: Found valid opt or flag
DEBUG:clap:Parser::parse_short_arg:iter:A: p[0]=[], p[1]=[116, 119, 111]
DEBUG:clap:Parser::parse_short_arg:iter:A: val=[116, 119, 111] (bytes), val="two" (ascii)
DEBUG:clap:Parser::parse_opt; opt=global-opt, val=Some("two")
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL | DELIM_NOT_SET | MULTIPLE_VALS)
DEBUG:clap:Parser::parse_opt; Checking for val...Found - "two", len: 3
DEBUG:clap:Parser::parse_opt: "two" contains '='...false
DEBUG:clap:Parser::add_val_to_arg; arg=global-opt, val="two"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."two"
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:ArgMatcher::needs_more_vals: o=global-opt
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=4551335407501243259
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:ArgMatcher::needs_more_vals: o=global-opt
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:Parser::parse_opt: More arg vals not required...
DEBUG:clap:Parser:get_matches_with: After parse_short_arg ValuesDone
DEBUG:clap:Parser::remove_overrides;
DEBUG:clap:Parser::remove_overrides:iter:4551335407501243259;
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:global-opt: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:global-opt: doesn't have default vals
DEBUG:clap:Validator::validate_conflicts;
DEBUG:clap:Validator::gather_conflicts;
DEBUG:clap:Validator::gather_conflicts:iter:4551335407501243259;
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:Validator::validate_required: required=ChildGraph([]);
DEBUG:clap:Validator::gather_requirements;
DEBUG:clap:Validator::gather_requirements:iter:4551335407501243259;
DEBUG:clap:Validator::validate_required_unless;
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:4551335407501243259: vals=[
    "two",
]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_arg_num_vals: num_vals set...1
DEBUG:clap:Validator::validate_arg_values: arg="global-opt"
DEBUG:clap:Validator::validate_arg_requires:global-opt;
DEBUG:clap:Validator::validate_arg_num_occurs: global-opt=1;
DEBUG:clap:Parser::remove_overrides;
DEBUG:clap:Parser::remove_overrides:iter:4551335407501243259;
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:global-opt: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:global-opt: doesn't have default vals
DEBUG:clap:Validator::validate_conflicts;
DEBUG:clap:Validator::gather_conflicts;
DEBUG:clap:Validator::gather_conflicts:iter:4551335407501243259;
DEBUG:clap:Parser::groups_for_arg: name=4551335407501243259
DEBUG:clap:Validator::validate_required: required=ChildGraph([]);
DEBUG:clap:Validator::gather_requirements;
DEBUG:clap:Validator::gather_requirements:iter:4551335407501243259;
DEBUG:clap:Validator::validate_required_unless;
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:4551335407501243259: vals=[
    "one",
]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_arg_num_vals: num_vals set...1
DEBUG:clap:Validator::validate_arg_values: arg="global-opt"
DEBUG:clap:Validator::validate_arg_requires:global-opt;
DEBUG:clap:Validator::validate_arg_num_occurs: global-opt=1;
DEBUG:clap:ArgMatcher::get_global_values: global_arg_vec=[4551335407501243259]
</code>
</pre>
</details>


---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-01 12:06_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Label `D: hard` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 12:07_

---

_Comment by @yaahc on 2020-02-04 23:56_

not sure if this is the same issue or a different issue but I'm seeing something similar where a global arg isn't propogating to the `ArgMatches.subcommand()` matches properly

```rust
use clap::{App, Arg};

fn main() {
    let matches = App::new("myapp")
        .subcommand(
            App::new("test")
                .arg(
                    Arg::with_name("global-opt")
                        .number_of_values(1)
                        .short('A')
                        .global(true),
                )
                .subcommand(App::new("testt")),
        )
        .get_matches_from(vec!["myapp", "test", "testt", "-Aone"]);

    let gs = matches.value_of("global-opt");
    assert_eq!(gs, Some("one"));
}
```

The assert_eq here fails.

---

_Comment by @pksunkara on 2020-02-05 07:54_

It is the same issue. Thanks for the reports guys. Unfortunately we won't get to fix this until 3.0 is out.

---

_Referenced in [getsentry/sentry-cli#724](../../getsentry/sentry-cli/pulls/724.md) on 2020-04-29 19:28_

---

_Comment by @kamilogorek on 2021-04-21 13:16_

Hey, is this change still in the v3 roadmap?

---

_Comment by @pksunkara on 2021-04-21 14:37_

If you look at the milestone. It's not scheduled for v3 release. But afterwards.

---

_Referenced in [drogue-iot/drg#70](../../drogue-iot/drg/issues/70.md) on 2021-05-19 13:13_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#128](../../epage/clapng/issues/128.md) on 2021-12-06 18:38_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:06_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:06_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 20:19_

---

_Label `E-help-wanted` removed by @epage on 2021-12-09 20:19_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 20:19_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 20:20_

---

_Comment by @epage on 2022-11-08 05:21_

#3938 is a related issue for how to deal with global args appearing before and after a subcommand

---

_Referenced in [clap-rs/clap#5588](../../clap-rs/clap/issues/5588.md) on 2024-07-19 16:06_

---
